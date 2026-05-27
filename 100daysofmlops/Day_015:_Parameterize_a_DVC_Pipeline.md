## Instruction

The xFusionCorp Industries ML team manages model hyperparameters using `params.yaml` so experiments can be modified without changing source code.

The `fraud-detection` project already contained a DVC pipeline with a `train` stage configured to use:

```yaml id="y2n7k1"
params:
  - n_estimators
```

However, running:

```bash id="k8m4x2"
dvc repro
```

failed because the parameter configuration was incorrect.

The task was to:

* Fix the parameter wiring
* Successfully run the pipeline
* Demonstrate that DVC detects parameter changes
* Verify only the training stage reruns when the parameter changes



# Existing Configuration

## params.yaml

```yaml id="p5v9m3"
n_estimator: 100
```

## train Stage

```yaml id="f3m8x7"
train:
  cmd: python src/models/train.py
  deps:
    - data/processed/train.csv
    - src/models/train.py
  params:
    - n_estimators
  outs:
    - models/model.pkl
```


# Solution

## Step 1 - Fix params.yaml

Update:

```yaml id="c2m7x4"
n_estimator: 100
```

to:

```yaml id="v8m1x6"
n_estimators: 100
```

Final file:

```yaml id="n5x3m8"
n_estimators: 100
```

Now the parameter name matches the reference in `dvc.yaml`.

<img width="860" height="676" alt="dvc" src="https://github.com/user-attachments/assets/e3c321b8-63df-4af3-a223-ad0bf39edf09" />

<img width="286" height="112" alt="prams" src="https://github.com/user-attachments/assets/18a0fe44-b949-4608-b0a2-ee0716a1f749" />


# Step 2 - Run the Full Pipeline

Navigate to the project:

```bash id="p9m2x5"
cd /root/code/fraud-detection
```

Run:

```bash id="j4x8m1"
dvc repro
```

<img width="631" height="477" alt="1dvcrun" src="https://github.com/user-attachments/assets/71cfcfe5-33a3-4235-a655-9897e06cbce5" />


Expected execution order:

```text id="a7m3x9"
process_data
     ↓
split_data
     ↓
train
```

DVC should successfully generate:

```bash id="w2m6x4"
models/model.pkl
```

<img width="637" height="492" alt="2dvcrun" src="https://github.com/user-attachments/assets/05a2942c-3e55-429f-9e44-0d5a6b709520" />


# Step 3 - Verify Pipeline Status

```bash id="k7x1m8"
dvc status
```

Expected output:

```text id="r5m9x2"
Data and pipelines are up to date.
```



# Demonstrating Parameter Tracking

## Step 4 - Modify the Hyperparameter

Update:

```yaml id="t3m7x5"
n_estimators: 100
```

to:

```yaml id="y8m2x1"
n_estimators: 200
```



## Step 5 - Re-run the Pipeline

Execute:

```bash id="g6x4m9"
dvc repro
```

Expected result:

```text id="z2m8x7"
Running stage 'train'
```

Only the **train** stage should execute.

The following stages should NOT rerun:

```text id="n4x1m6"
process_data
split_data
```

because neither their dependencies nor outputs changed.



# Step 6 - Verify dvc.lock

Inspect:

```bash id="p1m7x3"
cat dvc.lock
```

Expected train stage section:

```yaml id="v5x8m2"
params:
  params.yaml:
    n_estimators: 200
```

This proves DVC recorded the new hyperparameter value.



# Step 7 - Verify Model Regeneration

Check:

```bash id="r9m2x4"
ls -l models/model.pkl
```

The model artifact should have a newer timestamp, confirming retraining occurred.



# Understanding the Concept 💡

## What is params.yaml?

`params.yaml` stores experiment parameters outside of code.

Example:

```yaml id="c8x3m1"
n_estimators: 200
learning_rate: 0.01
```

This makes experiments reproducible and configurable.



## Why Use Parameters Instead of Hardcoding?

Bad:

```python id="f7m2x6"
n_estimators = 200
```

Good:

```yaml id="m3x8v5"
n_estimators: 200
```

Benefits:

* Easier experimentation
* Better reproducibility
* No code changes required



## How Does DVC Track Parameters?

DVC records parameter values in:

```bash id="q4m7x2"
dvc.lock
```

When a parameter changes:

* DVC detects it
* Only affected stages rerun
* Unnecessary work is avoided



# Fun Message 😄

*"Changing one number and retraining only what matters—that's the difference between MLOps and chaos 😄"*


