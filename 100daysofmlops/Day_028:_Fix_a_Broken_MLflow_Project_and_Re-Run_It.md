## Instruction

The xFusionCorp Industries ML Platform team packages training workflows as **MLflow Projects** so that any engineer can reproduce a training run with a single command.

A project was pre-staged at:

```bash
/root/code/trainer/
```

However, the initial automated run triggered during lab startup failed because of a bug in the:

```bash
MLproject
```

file.

The objective was to:

* Diagnose the failed MLflow Project run
* Fix the command-line bug in `MLproject`
* Successfully execute the project twice
* Verify the runs were recorded in MLflow


# Solution

## ✅ Fix MLproject

Update:

```bash
--n_est
```

to:

```bash
--n_estimators
```

Correct version:

```yaml
entry_points:
  train:
    parameters:
      n_estimators: {type: int, default: 100}
      max_depth: {type: int, default: 5}
    command: >
      python train.py
      --n_estimators {n_estimators}
      --max_depth {max_depth}
```

<img width="782" height="470" alt="fix" src="https://github.com/user-attachments/assets/e2145954-1978-4b7b-9af6-545767d5366a" />

# Step-by-Step Execution

## 1. Navigate to Project

```bash
cd /root/code/trainer
```



## 2. Edit MLproject

```bash
vi MLproject
```

Replace:

```bash
--n_est
```

with:

```bash
--n_estimators
```

Save.



## 3. Run Project with Custom Parameters

```bash
mlflow run . \
-e train \
-P n_estimators=200 \
-P max_depth=10 \
--env-manager=local
```

Expected:

```text
Run completed successfully
```

<img width="755" height="181" alt="1st run" src="https://github.com/user-attachments/assets/39eb9344-62ef-4e8a-b6e1-81b3dcd61341" />


## 4. Run Project with Default Parameters

```bash
mlflow run . \
-e train \
--env-manager=local
```

Expected defaults:

```text
n_estimators=100
max_depth=5
```

<img width="767" height="107" alt="2nd run" src="https://github.com/user-attachments/assets/94fd537e-a38d-4ef5-b346-24cb9512bac4" />


# Verification

## Verify MLflow UI

Open:

```text
MLflow UI
```

Navigate to:

```text
trainer
```

experiment.

Expected:

### Failed Run

```text
Status: FAILED
```

The original startup run remains present.

<img width="777" height="517" alt="uiruns" src="https://github.com/user-attachments/assets/ef13d1c4-d7c0-4cb4-82a3-eaa7da9b850b" />


### Successful Run #1

Parameters:

```text
n_estimators = 200
max_depth = 10
```

Status:

```text
FINISHED
```

<img width="755" height="181" alt="1st run" src="https://github.com/user-attachments/assets/d12239d0-cbc6-427f-80b1-c8d46dc9ef79" />


### Successful Run #2

Parameters:

```text
n_estimators = 100
max_depth = 5
```

Status:

```text
FINISHED
```

<img width="767" height="107" alt="2nd run" src="https://github.com/user-attachments/assets/2ecd2ca7-d600-43b5-9f51-48dcdafda49f" />


# Verify from CLI

List experiment runs:

```bash
mlflow experiments search
```

or inspect the UI.

You should observe:

| Run         | Status   | n_estimators |
| ----------- | -------- | ------------ |
| Startup Run | FAILED   | N/A          |
| Run 1       | FINISHED | 200          |
| Run 2       | FINISHED | 100          |


# Fun Message 😄

*"One missing 'imators' in --n_estimators was all it took to turn an ML pipeline into a debugging exercise 😄🔍"*
