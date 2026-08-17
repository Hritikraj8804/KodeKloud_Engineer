# Instruction

The xFusionCorp Industries MLOps team is building the serving path for the **fraud-detector** platform.

The workflow consists of:

* Training a fraud-detection model using the seeded transaction dataset
* Logging the training run and artifacts to **MLflow**
* Registering the trained model as `fraud-detector`
* Assigning the `production` alias to the registered model version
* Starting the FastAPI inference server
* Serving predictions through `models:/fraud-detector@production`

The provided `register.py` scaffold already contains the MLflow connection and latest-run lookup. The registration and promotion logic was left as a **TODO**.

# Solution

## Final `register.py`

```python
"""Register the trained run as `fraud-detector` and promote it.

train.py logs a run to the `fraud-detection` experiment. This script
turns that run into the model the serving layer targets: it registers
`runs:/<run_id>/model` as a version of `fraud-detector`, then puts the
`production` alias on that version.
"""

from __future__ import annotations

import mlflow

TRACKING_URI = "http://localhost:5000"
MODEL_NAME = "fraud-detector"
ALIAS = "production"

mlflow.set_tracking_uri(TRACKING_URI)
client = mlflow.tracking.MlflowClient()


def _latest_run_id() -> str:
    """The most recent run id in the `fraud-detection` experiment."""
    exp = client.get_experiment_by_name("fraud-detection")

    if exp is None:
        raise SystemExit(
            "no `fraud-detection` experiment yet -- run train.py first"
        )

    runs = client.search_runs(
        [exp.experiment_id],
        order_by=["attributes.start_time DESC"],
        max_results=1,
    )

    if not runs:
        raise SystemExit(
            "no runs in `fraud-detection` -- run train.py first"
        )

    return runs[0].info.run_id


run_id = _latest_run_id()
print(f"[register] latest run_id={run_id}")

# Register the model artifact from the latest MLflow run
model_uri = f"runs:/{run_id}/model"

model_version = mlflow.register_model(
    model_uri=model_uri,
    name=MODEL_NAME,
)

# Promote the newly registered version
client.set_registered_model_alias(
    MODEL_NAME,
    ALIAS,
    model_version.version,
)

print(
    f"[register] promoted fraud-detector version "
    f"{model_version.version} to {ALIAS}"
)
```

# Solution Steps

## Step 1 - Navigate to the Project

```bash
cd /root/code
```

## Step 2 - Run the Training Script

The training script is already complete and should not be modified.

```bash
python3 /root/code/train.py
```

This creates the:

```text
fraud-detection
```

MLflow experiment and logs a training run.

The model artifact is logged under:

```text
model
```

Expected flow:

```text
transactions.csv
      │
      ▼
   train.py
      │
      ├── MLflow parameters
      ├── MLflow metrics
      └── model artifact
              │
              ▼
       MLflow + SeaweedFS
```

<img width="1437" height="260" alt="train run " src="https://github.com/user-attachments/assets/cd709f17-00be-4cf1-8821-5b8b5ae947c2" />

## Step 3 - Edit `register.py`

```bash
vi /root/code/register.py
```

Replace the TODO with:

```python
model_uri = f"runs:/{run_id}/model"

model_version = mlflow.register_model(
    model_uri=model_uri,
    name=MODEL_NAME,
)

client.set_registered_model_alias(
    MODEL_NAME,
    ALIAS,
    model_version.version,
)

print(
    f"[register] promoted fraud-detector version "
    f"{model_version.version} to {ALIAS}"
)
```

## Step 4 - Register and Promote the Model

```bash
python3 /root/code/register.py
```

Expected output will be similar to:

```text
[register] latest run_id=<run-id>
Successfully registered model 'fraud-detector'
[register] promoted fraud-detector version 1 to production
```

<img width="1452" height="221" alt="registry run" src="https://github.com/user-attachments/assets/bf44fdd0-a4d9-4857-9f52-8f4c140d49e1" />

The exact run ID and version may differ.

# Verification

## Verify MLflow Experiment

Open the **MLflow UI** from the lab.

Check:

```text
Experiments
└── fraud-detection
    └── Run
```

The experiment should contain at least one run.

## Verify Registered Model

In MLflow:

```text
Models
└── fraud-detector
```

The model should have a registered version.

Expected:

```text
fraud-detector
└── Version 1
    └── Alias: production
```

## Verify Production Alias

The critical configuration is:

```text
fraud-detector
        │
        ▼
    version 1
        │
        ▼
   production
```

The serving application resolves:

```text
models:/fraud-detector@production
```

rather than:

```text
models:/fraud-detector/1
```

This means future model promotion can simply move the alias.

# Start the FastAPI Server

Run:

```bash
python3 /root/code/serve.py
```

The server should start on port:

```text
8085
```

The background loader should eventually resolve:

```text
models:/fraud-detector@production
```

and load the registered model.

<img width="1417" height="162" alt="serve run" src="https://github.com/user-attachments/assets/be33be5f-7dfc-474d-b91c-b43615ea9f47" />

# Verify Prediction API

Send the required request:

```bash
curl -X POST http://localhost:8085/predict \
  -H "Content-Type: application/json" \
  -d '{"features":[100.5,12,3]}'
```

Expected:

```json
{"prediction":0}
```

or:

```json
{"prediction":1}
```

The exact prediction depends on the deterministic model generated by the training script.

<img width="1028" height="112" alt="prediction" src="https://github.com/user-attachments/assets/f910d15b-fe83-4538-b5c8-e7eac5041279" />

<img width="931" height="732" alt="checks" src="https://github.com/user-attachments/assets/903088eb-96e5-42c2-9dda-4f3e790554ad" />

<img width="1547" height="691" alt="mlflow" src="https://github.com/user-attachments/assets/d59d92b4-966e-4ba7-928e-7c43badd5f74" />

# Fun Message 😄

*"Training creates the model, the registry gives it an identity, and the production alias tells the serving layer which model is allowed to go live. 🚀🤖"*
