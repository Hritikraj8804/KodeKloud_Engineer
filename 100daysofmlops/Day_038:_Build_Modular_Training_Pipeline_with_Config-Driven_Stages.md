# Instruction

The xFusionCorp Industries ML Platform team wanted to compare the performance difference between:

* Single-threaded Random Forest training
* Fully parallel Random Forest training

The experiment was tracked in MLflow so engineers could compare wall-clock training times and quantify the speedup.

A draft script already existed:

```bash
/root/code/fraud-detection/src/models/train_parallel.py
```

However, two problems prevented the bake-off from working correctly:

1. Both runs used the same number of workers.
2. MLflow logged the same `n_jobs` value for both runs.

As a result:

* Training times were nearly identical.
* MLflow Compare View could not distinguish the runs.

# Project Structure

```text
fraud-detection/
├── data/
│   └── train.csv
├── models/
│   └── model.pkl
└── src/
    └── models/
        └── train_parallel.py
```

# Required Code Changes

## Fix #1

Replace:

```python
N_JOBS_VALUES = [1, 1]
```

with:

```python
N_JOBS_VALUES = [1, -1]
```

## Fix #2

Replace:

```python
mlflow.log_param("n_jobs", "all")
```

with:

```python
mlflow.log_param("n_jobs", n_jobs)
```

# Final Corrected Snippets

## Parallel Configuration

```python
N_JOBS_VALUES = [1, -1]
```

## MLflow Logging

```python
mlflow.log_param("n_jobs", n_jobs)
mlflow.log_param("n_estimators", N_ESTIMATORS)
```

# Solution Steps

## Step 1 - Edit Script

```bash
vi /root/code/fraud-detection/src/models/train_parallel.py
```

Apply both fixes.

## Step 2 - Run the Experiment

```bash
python3 /root/code/fraud-detection/src/models/train_parallel.py
```

Expected output:

```text
[serial] n_jobs=1 training_time_seconds=4.532

[parallel] n_jobs=-1 training_time_seconds=2.917

Model saved to /root/code/fraud-detection/models/model.pkl
```

<img width="737" height="225" alt="run" src="https://github.com/user-attachments/assets/7fa8d377-faff-4f9b-b9d7-577ab0595b0c" />

*(times will vary depending on CPU resources)*

# Verification

## Verify Model File

```bash
ls -l /root/code/fraud-detection/models/model.pkl
```

Expected:

```text
model.pkl
```

exists.

## Verify MLflow Runs

Open:

```text
MLflow UI
→ parallel-training
```

Expected:

```text
2 Runs
```

### Run 1

Parameters:

```text
n_jobs = 1
n_estimators = 200
```

Metrics:

```text
training_time_seconds
```

### Run 2

Parameters:

```text
n_jobs = -1
n_estimators = 200
```

Metrics:

```text
training_time_seconds
```

## Verify Speedup

Compare:

```text
serial_time
parallel_time
```

Requirement:

```text
parallel_time
<
serial_time × 0.90
```

At least:

```text
10% faster
```

# Example

| Run         | n_jobs | Training Time |
| ----------- | ------ | ------------- |
| serial      | 1      | 4.50 s        |
| parallel    | -1     | 2.90 s        |
| Improvement | —      | 35.5%         |

<img width="1767" height="450" alt="ui" src="https://github.com/user-attachments/assets/18786115-a908-42ef-ae70-2e00e9ad4a18" />

# Fun Message 😄

*"Training 200 trees on one CPU is dedication. Training them on all CPUs is delegation 😄🌲⚡"*
