# Instruction

The xFusionCorp Industries ML Platform team evaluates fraud-detection models using **5-fold cross-validation** to ensure model performance is measured across multiple data splits.

Since the fraud dataset is imbalanced (approximately **70% non-fraud / 30% fraud**), the team requires a **stratification-aware splitter** so every fold preserves the original class distribution.

A draft cross-validation scaffold existed at:

```bash
/root/code/fraud-detection/src/models/cross_validate.py
```

However, two issues prevented it from satisfying the release checklist:

1. The cross-validation splitter was not stratified.
2. The aggregate report schema did not match the required format.

The task was to correct both issues and generate a valid cross-validation report.


# Solution Steps

## Step 1 - Edit cross_validate.py

```bash
vi /root/code/fraud-detection/src/models/cross_validate.py
```


## Step 2 - Fix the Splitter

Replace the existing CV object with:

```python
cv = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```


## Step 3 - Fix Aggregate Results

Update the final report dictionary to:

```python
results = {
    "mean_accuracy": ...,
    "std_accuracy": ...,
    "mean_f1": ...,
    "std_f1": ...,
    "mean_roc_auc": ...,
    "std_roc_auc": ...,
    "folds": fold_results,
}
```


## Step 4 - Run the Evaluation

```bash
python3 /root/code/fraud-detection/src/models/cross_validate.py
```

Expected:

```text
Cross validation complete
```

(or similar output)


# Verification

## Verify Report Exists

```bash
ls -l /root/code/fraud-detection/reports/cv_results.json
```

Expected:

```text
cv_results.json
```


## Inspect Report

```bash
cat /root/code/fraud-detection/reports/cv_results.json
```

Expected structure:

```json
{
  "mean_accuracy": 0.91,
  "std_accuracy": 0.03,
  "mean_f1": 0.88,
  "std_f1": 0.02,
  "mean_roc_auc": 0.94,
  "std_roc_auc": 0.01,
  "folds": [
    {
      "fold": 1,
      "accuracy": 0.90,
      "f1": 0.87,
      "roc_auc": 0.93
    }
  ]
}
```


## Verify Number of Folds

```bash
jq '.folds | length' \
/root/code/fraud-detection/reports/cv_results.json
```

Expected:

```text
5
```

<img width="172" height="262" alt="report" src="https://github.com/user-attachments/assets/dc9e3a39-5ba7-4f90-b6ef-0d93ccdbf095" />

## Verify MLflow

Open:

```text
MLflow UI
→ fraud-detection-cv
```

Expected:

### Parent Run

```text
cross-validation
```


### Nested Runs

```text
fold-1
fold-2
fold-3
fold-4
fold-5
```

Each fold should contain:

```text
accuracy
f1
roc_auc
```

metrics.

<img width="1821" height="877" alt="ui" src="https://github.com/user-attachments/assets/f2b9fa63-e813-4f3b-a768-b98dd434217a" />

# Fun Message 😄

*"A single train-test split tells you how your model performed once. Cross-validation tells you whether it got lucky 😄📊"*
