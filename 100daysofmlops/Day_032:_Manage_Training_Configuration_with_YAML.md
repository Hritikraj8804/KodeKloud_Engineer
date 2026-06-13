## Instruction

The xFusionCorp Industries ML Platform team requires strict reproducibility for audit and compliance purposes.

The fraud detection training pipeline was producing different results every time it was executed, even though:

* The code was unchanged
* The dataset was unchanged
* The hyperparameters were unchanged

This broke the team's reproducibility checks.

The goal was to modify the trainer so that identical inputs always produced identical:

* Accuracy
* F1 Score
* Feature Importances

allowing the determinism probe to pass successfully.


# Solution

Use a fixed random seed everywhere randomness occurs.

A common value:

```python
42
```

# Required Changes

## Fix Train/Test Split

Replace:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y
)
```

with:

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    stratify=y,
    random_state=42
)
```

## Fix RandomForest

Replace:

```python
model = RandomForestClassifier(
    n_estimators=100,
    max_depth=5
)
```

with:

```python
model = RandomForestClassifier(
    n_estimators=100,
    max_depth=5,
    random_state=42
)
```

# Final Code Changes

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    stratify=y,
    random_state=42
)

model = RandomForestClassifier(
    n_estimators=100,
    max_depth=5,
    random_state=42
)
```

<img width="960" height="667" alt="config" src="https://github.com/user-attachments/assets/d175787c-98c4-425c-b538-04b75552b1b3" />

## Step 2 - Run Determinism Check

```bash
/root/code/fraud-detection/check_determinism.sh
```

Expected:

```text
PASS
```

<img width="767" height="547" alt="run" src="https://github.com/user-attachments/assets/62cf3456-b572-4a00-ae3d-aa73cc6813b7" />

## Step 3 - Verify Exit Code

```bash
echo $?
```

Expected:

```text
0
```

<img width="421" height="102" alt="verify cli" src="https://github.com/user-attachments/assets/7d887cf6-7ea2-4bc6-92dd-1a89e7883a32" />

# Validation

## Verify Metrics Files

```bash
diff \
reports/metrics_run_1.json \
reports/metrics_run_2.json
```

Expected:

```text
(no output)
```

```bash
diff \
reports/metrics_run_2.json \
reports/metrics_run_3.json
```

Expected:

```text
(no output)
```

## Verify MLflow Runs

Open:

```text
MLflow UI
→ fraud-detection-repro
```

Expected:

```text
repro-run-1
repro-run-2
```

Both runs should contain identical:

```text
accuracy
f1_score
```

values.

Example:

```text
accuracy = 0.925000
f1_score = 0.909091
```

<img width="1746" height="497" alt="verify ui" src="https://github.com/user-attachments/assets/966a0a00-8076-4c4a-b43e-146ed17076b8" />

*(actual values depend on the dataset)*

# Fun Message 😄

*"A model that changes its mind every run is creative. A model that gives the same answer every run is production-ready 😄🔁"*
