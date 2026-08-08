# Instruction

The xFusionCorp Industries ML Platform team is piloting **Prefect 3.x** alongside Argo Workflows for ML pipeline orchestration.

The existing `fraud-pipeline` deployment contains three logical stages:

```text
prep → train → evaluate
```

However, the Prefect Flow Run graph only displayed:

```text
prep → train
```

The `evaluate` function was executing, but it was not decorated as a Prefect task. Therefore, Prefect did not create a task-run record for it.

The objective was to:

* Convert `evaluate` into a Prefect task.
* Preserve the task name as `evaluate`.
* Redeploy the flow.
* Trigger a new Quick Run from the Prefect UI.
* Verify all three task runs reach `Completed`.


# Solution

## Final `fraud_pipeline.py`

```python
"""Prefect 3.x flow for the fraud-detection pipeline.

Structure: prep -> train -> evaluate. Each step is a `@task`-decorated
function -- or should be. Inspect the evaluate node on the Prefect UI's
Flow Run graph to see whether it is being tracked.
"""

from __future__ import annotations

from prefect import flow, task


@task(name="prep")
def prep() -> dict:
    print("[prep] preparing training data")
    return {"rows": 100, "path": "/tmp/train.csv"}


@task(name="train")
def train(data: dict) -> str:
    print(f"[train] fitting model on {data['rows']} rows from {data['path']}")
    return "model-v1"


@task(name="evaluate")
def evaluate(model: str) -> float:
    print(f"[evaluate] scoring model {model}")
    return 0.75


@flow(name="fraud-pipeline")
def fraud_pipeline() -> float:
    data = prep()
    model = train(data)
    score = evaluate(model)
    print(f"[flow] final score={score}")
    return score


if __name__ == "__main__":
    # `.serve()` registers a deployment named `fraud-pipeline` and
    # blocks as a worker loop, picking up Quick Run triggers from the
    # Prefect UI.
    fraud_pipeline.serve(name="fraud-pipeline", tags=["lab"])
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/prefect
```

## Step 2 - Edit Flow

```bash
vi fraud_pipeline.py
```

Add the missing decorator:

```python
@task(name="evaluate")
```

directly above the `evaluate()` function.

## Step 3 - Redeploy Prefect

The provided Makefile handles the existing Prefect serve process.

```bash
make restart
```

<img width="690" height="75" alt="reployed" src="https://github.com/user-attachments/assets/076510b0-3ca3-4957-a13c-41626813599b" />

<img width="727" height="255" alt="perfect deployment" src="https://github.com/user-attachments/assets/d8ca2fcd-c64b-4894-b60e-8adad68eb115" />

If the Makefile exposes a different restart target, use the target provided by the scaffold.

The serve log can be checked with:

```bash
tail -f /var/log/prefect-serve.log
```

Verify the new Prefect process is running.

## Step 4 - Open Prefect UI

Open:

```text
http://localhost:5000
```

Navigate to:

```text
Deployments
```

Find:

```text
fraud-pipeline
```

## Step 5 - Trigger Quick Run

Click:

```text
Quick Run
```

Open the newly created Flow Run.

<img width="732" height="357" alt="run cli check" src="https://github.com/user-attachments/assets/ed59eed1-8e5b-4923-ac7a-87b903a37012" />

# Verification

## Flow Run Graph

The graph should now contain:

```text
prep
  │
  ▼
train
  │
  ▼
evaluate
```

All three nodes should reach:

```text
Completed
```

✔

<img width="1660" height="852" alt="run ui check" src="https://github.com/user-attachments/assets/2fba7bff-cc97-4dd8-bb21-5e1659bca86f" />

# Workflow

```text
Prefect Deployment
        │
        ▼
     Quick Run
        │
        ▼
      Flow
        │
        ▼
      prep
        │
        ▼
      train
        │
        ▼
    evaluate
        │
        ▼
   score = 0.75
        │
        ▼
    Completed
```

# Before vs After

### Before

```text
Flow
 │
 ├── prep       ✓
 │
 ├── train      ✓
 │
 └── evaluate   ✗ not tracked
```

`evaluate()` executed as a regular Python function.

### After

```text
Flow
 │
 ├── prep       ✓ Completed
 │
 ├── train      ✓ Completed
 │
 └── evaluate   ✓ Completed
```

# Fun Message 😄

*"If the orchestrator can't see the step, it's basically the invisible intern of your pipeline. Decorate it with `@task` and Prefect finally knows it exists! 👻➡️📊"*
