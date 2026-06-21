# Instruction

The xFusionCorp Industries ML Platform team built a complete fraud-detection training pipeline consisting of five stages:

```text
Validate Data
      ↓
Hyperparameter Tuning
      ↓
Model Selection
      ↓
Model Registration
      ↓
Training Report
```

Everything could be executed through a single command:

```bash
make train-pipeline
```

However, the pipeline failed due to multiple wiring issues across:

* Makefile
* `src/select_model.py`
* `src/register.py`

The goal was to fix the pipeline so it could run end-to-end and automatically:

* Validate data
* Run Optuna tuning
* Select the best model
* Register the model in MLflow Model Registry
* Assign the correct release alias
* Generate a consolidated training report

# Solution Steps

## Step 1 - Fix Makefile

```bash
vi Makefile
```

## Step 2 - Fix Model Selection

```bash
vi src/select_model.py
```

Update metric references from:

```text
accuracy
```

to:

```text
f1_score
```

## Step 3 - Fix Registration Alias

```bash
vi src/register.py
```

Change:

```python
RELEASE_ALIAS = "staging"
```

## Step 4 - Run Pipeline

```bash
cd /root/code/fraud-detection

make train-pipeline
```

Expected:

```text
[validate] OK
[tune] completed
[select] ...
[register] ...
[report] ...
```

<img width="737" height="275" alt="run" src="https://github.com/user-attachments/assets/a5d8f0d8-0d49-4ffa-a3ff-3739a70fa4bc" />

Pipeline completes successfully.

# Verification

## Verify Validation Report

```bash
cat reports/validation_status.json
```

Expected:

```json
{
  "status": "ok"
}
```

## Verify Selection

```bash
cat reports/selection.json
```

Expected:

```json
{
  "run_id": "...",
  "model_type": "...",
  "f1_score": 0.8
}
```

## Verify Final Training Report

```bash
cat reports/training_report.json
```

Expected keys:

```text
best_model
best_params
metrics
total_trials
validation_status
```

## Verify MLflow Experiment

Open:

```text
MLflow UI
→ fraud-detection-tuning
```

Expected:

```text
≥ 5 runs
```

Each run contains:

```text
params.n_estimators
params.max_depth
metrics.f1_score
```

<img width="1890" height="887" alt="ui" src="https://github.com/user-attachments/assets/87dec758-6180-4449-ba83-d03db51e432f" />

# Fun Message 😄

*"An ML pipeline is like a relay race — if tuning hands the baton to selection before it starts running, nobody reaches the finish line 😄🏃‍♂️🏁"*
