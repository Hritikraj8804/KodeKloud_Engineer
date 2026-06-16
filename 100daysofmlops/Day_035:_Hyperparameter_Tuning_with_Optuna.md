# Instruction

The xFusionCorp Industries ML Platform team uses **Optuna** for hyperparameter optimization and **MLflow** to track every trial.

A tuning scaffold already existed at:

```bash
/root/code/fraud-detection/src/models/tune.py
```

The objective was to:

* Run a 20-trial hyperparameter search
* Track every trial in MLflow
* Save the best configuration to YAML
* Ensure the selected configuration corresponds to the **highest F1 score**, not the lowest

However, the scaffold contained two critical issues:

1. The Optuna study optimized in the wrong direction.
2. No trial information was logged to MLflow.



# Final Objective Function

```python
def objective(trial, X, y):
    n_estimators = trial.suggest_int(
        "n_estimators", 50, 500
    )

    max_depth = trial.suggest_int(
        "max_depth", 3, 20
    )

    model = RandomForestClassifier(
        n_estimators=n_estimators,
        max_depth=max_depth,
        random_state=42,
    )

    cv = StratifiedKFold(
        n_splits=N_SPLITS,
        shuffle=True,
        random_state=42,
    )

    scores = cross_val_score(
        model,
        X,
        y,
        cv=cv,
        scoring="f1",
    )

    score = float(np.mean(scores))

    with mlflow.start_run():
        mlflow.log_param(
            "n_estimators",
            n_estimators,
        )
        mlflow.log_param(
            "max_depth",
            max_depth,
        )
        mlflow.log_metric(
            "f1_score",
            score,
        )

    return score
```


# Fix Study Creation

Replace:

```python
study = optuna.create_study(
    direction="minimize",
    study_name=EXPERIMENT_NAME
)
```

with:

```python
study = optuna.create_study(
    direction="maximize",
    study_name=EXPERIMENT_NAME
)
```


# Solution Steps

## Step 1 - Edit tune.py

```bash
vi /root/code/fraud-detection/src/models/tune.py
```

Apply both fixes.


## Step 2 - Run Hyperparameter Search

```bash
python3 /root/code/fraud-detection/src/models/tune.py
```

Expected:

```text
best params: {...}
best f1: 0.xxxxxx
wrote /root/code/fraud-detection/configs/best_params.yaml
```

<img width="765" height="412" alt="runs" src="https://github.com/user-attachments/assets/0760b10b-8983-4d52-aeb2-37da3bb908f5" />

# Verification

## Verify YAML File

```bash
cat /root/code/fraud-detection/configs/best_params.yaml
```

Expected:

```yaml
max_depth: 12
n_estimators: 300
```

Requirements:

```text
n_estimators → 50–500
max_depth → 3–20
```

Only these two keys should exist.

<img width="742" height="72" alt="best_param" src="https://github.com/user-attachments/assets/e6e7cfa7-1e19-4144-ad22-6cfb71d00392" />

## Verify MLflow Experiment

Open:

```text
MLflow UI
→ hyperopt-tuning
```

Expected:

```text
20 runs
```

(or more if rerun)

<img width="1777" height="886" alt="verify ui" src="https://github.com/user-attachments/assets/87a7d6a5-9727-4ca0-be21-a068f542cc3d" />

## Verify Logged Parameters

Each run contains:

```text
n_estimators
max_depth
```

under Parameters.


## Verify Logged Metrics

Each run contains:

```text
f1_score
```

under Metrics.


## Verify Best Trial

The run corresponding to:

```yaml
best_params.yaml
```

should have the highest:

```text
f1_score
```

<img width="742" height="72" alt="best_param" src="https://github.com/user-attachments/assets/c0e4de7a-92c5-4a14-aa13-c6f54f038fba" />

among all trials.

# Fun Message 😄

*"An optimizer that minimizes F1 is technically working—it’s just helping you find the worst model possible 😄🎯"*
