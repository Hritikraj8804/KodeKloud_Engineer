## Instruction

The xFusionCorp Industries ML Platform team requires every candidate model to pass a release checklist before promotion.

The checklist requires:

* Five evaluation metrics
* A confusion matrix image
* MLflow tracking
* Metrics stored inside the project reports directory

A draft evaluator already existed at:

```bash
/root/code/fraud-detection/src/models/evaluate.py
```

However, the generated report did not satisfy the required standards.

The task was to correct the evaluator and generate a complete evaluation report for the pre-trained fraud detection model.


# Correct Metrics Block

Replace:

```python
metrics = {
    "accuracy": round(accuracy_score(y, preds), 6),
    "f1": round(f1_score(y, preds), 6),
}
```

with:

```python
metrics = {
    "accuracy": round(accuracy_score(y, preds), 6),
    "precision": round(precision_score(y, preds), 6),
    "recall": round(recall_score(y, preds), 6),
    "f1_score": round(f1_score(y, preds), 6),
    "auc_roc": round(roc_auc_score(y, proba), 6),
}
```


# Final Required Changes

## Module-Level Constant

```python
METRICS_JSON = os.path.join(REPORTS_DIR, "metrics.json")
```

<img width="562" height="47" alt="json" src="https://github.com/user-attachments/assets/c390de17-d573-44a6-80a5-64e4d50f3c11" />

## Metrics Computation

```python
metrics = {
    "accuracy": round(accuracy_score(y, preds), 6),
    "precision": round(precision_score(y, preds), 6),
    "recall": round(recall_score(y, preds), 6),
    "f1_score": round(f1_score(y, preds), 6),
    "auc_roc": round(roc_auc_score(y, proba), 6),
}
```

<img width="547" height="272" alt="mertic fun" src="https://github.com/user-attachments/assets/aaed47da-c6f0-48b0-8ea1-af19127f0ba2" />

# Solution Steps

## Step 1 - Edit evaluate.py

```bash
vi /root/code/fraud-detection/src/models/evaluate.py
```

Apply the two fixes above.


## Step 2 - Execute Evaluation

```bash
python3 /root/code/fraud-detection/src/models/evaluate.py
```

Expected output:

```text
metrics: {...}
metrics JSON: /root/code/fraud-detection/reports/metrics.json
confusion matrix: /root/code/fraud-detection/reports/confusion_matrix.png
```

<img width="756" height="140" alt="run" src="https://github.com/user-attachments/assets/c847711b-1ebb-4989-a246-fece70ea3985" />

# Verification

## Verify Metrics JSON

```bash
cat /root/code/fraud-detection/reports/metrics.json
```

Expected structure:

```json
{
  "accuracy": 0.925,
  "auc_roc": 0.956,
  "f1_score": 0.909091,
  "precision": 0.909091,
  "recall": 0.909091
}
```

*(values will vary)*


## Verify Required Keys

Exactly these five keys:

```text
accuracy
precision
recall
f1_score
auc_roc
```

No extra keys.

## Verify Confusion Matrix

```bash
ls -l /root/code/fraud-detection/reports/confusion_matrix.png
```

Expected:

```text
confusion_matrix.png
```

exists.


## Verify MLflow Run

Open:

```text
MLflow UI
→ fraud-detection-eval
```

Expected:

```text
1 run
```


## Verify Logged Metrics

Under Metrics:

```text
accuracy
precision
recall
f1_score
auc_roc
```

<img width="1565" height="642" alt="verify ui" src="https://github.com/user-attachments/assets/eea7ba80-a30f-4d7d-9e6f-bcf69202180d" />

## Verify Logged Artifacts

Artifacts should contain:

```text
metrics.json
confusion_matrix.png
```


# Fun Message 😄

*"A model with only accuracy is like a student with only one exam score — the full report card tells the real story 😄📊"*

