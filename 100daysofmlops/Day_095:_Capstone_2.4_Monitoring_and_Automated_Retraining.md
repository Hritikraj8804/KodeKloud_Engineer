# Instruction

The xFusionCorp Industries MLOps team operates the **fraud-detector** model in production using MLflow.

The objective was to implement an automated retraining loop:

* Detect data drift
* Retrain only when drift is detected
* Register the newly trained model
* Move the `production` alias to the new version
* Confirm that version 2 becomes the production model

The provided `retrain_if_drift.py` scaffold already handled drift detection, retraining, and locating the latest `retrain` run. Two TODOs remained for the **drift gate** and **model promotion**.

# Solution

## Final `retrain_if_drift.py`

```python
"""Drift-triggered retraining for fraud-detector.

One command closes the loop: detect drift -> (only if drifted) retrain
on the combined data -> register the new run as a `fraud-detector`
version -> move the `production` alias to it.
"""

from __future__ import annotations

import json
import subprocess
import sys
from pathlib import Path

import mlflow

HERE = Path(__file__).resolve().parent
TRACKING_URI = "http://localhost:5000"
MODEL_NAME = "fraud-detector"
ALIAS = "production"

mlflow.set_tracking_uri(TRACKING_URI)
client = mlflow.tracking.MlflowClient()


def _run(script: str) -> None:
    """Run one of the provided scripts (drift.py / retrain.py)."""
    subprocess.check_call([sys.executable, str(HERE / script)])


def _latest_retrain_run_id() -> str:
    exp = client.get_experiment_by_name("fraud-detection")

    runs = client.search_runs(
        [exp.experiment_id],
        filter_string="tags.mlflow.runName = 'retrain'",
        order_by=["attributes.start_time DESC"],
        max_results=1,
    )

    if not runs:
        raise SystemExit("no `retrain` run found after retraining")

    return runs[0].info.run_id


# --- 1. Detect drift
_run("drift.py")

summary = json.loads(
    (HERE / "reports" / "drift-summary.json").read_text()
)

drifted = bool(summary.get("dataset_drift"))

print(f"[loop] dataset_drift={drifted}")

# Gate retraining on drift
if not drifted:
    print("[loop] no drift detected; retraining not needed")
    raise SystemExit(0)


# --- 2. Retrain on the combined data
_run("retrain.py")

run_id = _latest_retrain_run_id()

print(f"[loop] retrained: run_id={run_id}")


# --- 3. Register the retrained model
model_uri = f"runs:/{run_id}/model"

model_version = mlflow.register_model(
    model_uri=model_uri,
    name=MODEL_NAME,
)

# --- 4. Promote the new version
client.set_registered_model_alias(
    MODEL_NAME,
    ALIAS,
    model_version.version,
)

print(
    f"[loop] promoted fraud-detector version "
    f"{model_version.version} to {ALIAS}"
)
```


# Solution Steps

## Step 1 - Navigate to the Project

```bash
cd /root/code
```


## Step 2 - Edit the Retraining Script

```bash
vi /root/code/retrain_if_drift.py
```

Implement the two TODO sections.

### Drift Gate

```python
if not drifted:
    print("[loop] no drift detected; retraining not needed")
    raise SystemExit(0)
```

This ensures `retrain.py` is never executed when the dataset has not drifted.


### Register the Retrained Model

```python
model_uri = f"runs:/{run_id}/model"

model_version = mlflow.register_model(
    model_uri=model_uri,
    name=MODEL_NAME,
)
```


### Move the Production Alias

```python
client.set_registered_model_alias(
    MODEL_NAME,
    ALIAS,
    model_version.version,
)
```

This makes the newly registered version available through:

```text
models:/fraud-detector@production
```


# Step 3 - Run the Automated Loop

```bash
python3 /root/code/retrain_if_drift.py
```

<img width="745" height="387" alt="exec" src="https://github.com/user-attachments/assets/092106c8-ca47-4ea0-b2a7-f88bea29736c" />

Because the provided `current.csv` is shifted, the expected output starts with:

```text
[loop] dataset_drift=True
```

The script then runs the retraining process.

Expected output is similar to:

```text
[loop] dataset_drift=True
[loop] retrained: run_id=<run-id>
[loop] promoted fraud-detector version 2 to production
```

The exact run ID may differ.

<img width="745" height="326" alt="exec res" src="https://github.com/user-attachments/assets/f097c3f9-7e15-4e96-8875-84f9a6aa0edd" />

<img width="735" height="176" alt="report" src="https://github.com/user-attachments/assets/777962fe-af9c-43d5-b1ef-d56bf8fea880" />

<img width="1550" height="597" alt="mlflow ui" src="https://github.com/user-attachments/assets/9d49c4c5-3cb6-400f-96be-2878702e4c4e" />

# End-to-End Workflow

```text
             Production Data
                   │
                   ▼
              drift.py
                   │
                   ▼
        drift-summary.json
                   │
             dataset_drift?
              /         \
            No           Yes
            │             │
            ▼             ▼
          EXIT         retrain.py
                          │
                          ▼
                    MLflow Run
                    name=retrain
                          │
                          ▼
                  runs:/<run_id>/model
                          │
                          ▼
                  Model Registry
                   fraud-detector
                          │
                          ▼
                       Version 2
                          │
                          ▼
                production alias
                          │
                          ▼
                 Live production
```

# Fun Message 😄

*"Drift is the smoke alarm, retraining is the repair crew, and the production alias is the switch that puts the repaired model back online. 🔥🤖🚀"*
