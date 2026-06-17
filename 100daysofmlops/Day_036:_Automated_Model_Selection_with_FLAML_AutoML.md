# Instruction

The xFusionCorp Industries ML Platform team wanted to compare three fraud detection models and automatically select the best-performing candidate.

Three independent trainer scripts were already available:

```text
train_rf.py  → Random Forest
train_gb.py  → Gradient Boosting
train_lr.py  → Logistic Regression
```

Each trainer:

* Performs 3-fold stratified cross-validation
* Logs results to MLflow
* Tags the run with its model family
* Records the mean F1 score

The final step was handled by:

```bash
/root/code/fraud-detection/src/models/bakeoff.py
```

Unfortunately, the orchestrator contained two bugs:

1. It selected the worst model instead of the best.
2. The winner report was missing the model family.

The goal was to fix the orchestrator and generate a complete winner report.

# Project Structure

```text
fraud-detection/
├── data/
│   └── train.csv
├── reports/
│   └── winner.json
└── src/
    └── models/
        ├── train_rf.py
        ├── train_gb.py
        ├── train_lr.py
        └── bakeoff.py
```

# Step 1 - Run All Candidate Trainers

Before running the bake-off, execute all three training scripts:

```bash
python3 /root/code/fraud-detection/src/models/train_rf.py

python3 /root/code/fraud-detection/src/models/train_gb.py

python3 /root/code/fraud-detection/src/models/train_lr.py
```

<img width="756" height="317" alt="code run" src="https://github.com/user-attachments/assets/e66e14c3-fbb6-4ea9-b4a3-4f2aef88366d" />

This creates three runs inside the:

```text
bakeoff
```

MLflow experiment.

# Problem Analysis 🔍

## Bug #1 - Wrong Sorting Direction

Current code:

```python
runs = mlflow.search_runs(
    experiment_ids=[exp.experiment_id],
    order_by=["metrics.f1_score ASC"],
    max_results=10,
)
```

This sorts:

```text
Lowest F1
     ↓
Highest F1
```

Then:

```python
winner = runs.iloc[0]
```

selects:

```text
Worst Candidate
```

instead of:

```text
Best Candidate
```

### Fix

Replace:

```python
order_by=["metrics.f1_score ASC"]
```

with:

```python
order_by=["metrics.f1_score DESC"]
```

Now:

```python
winner = runs.iloc[0]
```

correctly selects the highest F1 score.

## Bug #2 - Missing model_type

Current report:

```python
report = {
    "run_id": winner["run_id"],
    "f1_score": float(winner["metrics.f1_score"]),
}
```

Required format:

```json
{
  "model_type": "random_forest",
  "run_id": "...",
  "f1_score": 0.91
}
```

The model family is stored in the MLflow tag:

```text
tags.candidate
```

### Fix

Add:

```python
"model_type": winner["tags.candidate"]
```

# Final Fix

Replace:

```python
runs = mlflow.search_runs(
    experiment_ids=[exp.experiment_id],
    order_by=["metrics.f1_score ASC"],
    max_results=10,
)
```

with:

```python
runs = mlflow.search_runs(
    experiment_ids=[exp.experiment_id],
    order_by=["metrics.f1_score DESC"],
    max_results=10,
)
```

Replace:

```python
report = {
    "run_id": winner["run_id"],
    "f1_score": float(winner["metrics.f1_score"]),
}
```

with:

```python
report = {
    "model_type": winner["tags.candidate"],
    "run_id": winner["run_id"],
    "f1_score": float(winner["metrics.f1_score"]),
}
```

# Corrected Report Section

```python
winner = runs.iloc[0]

report = {
    "model_type": winner["tags.candidate"],
    "run_id": winner["run_id"],
    "f1_score": float(winner["metrics.f1_score"]),
}
```

# Run the Bake-Off

Execute:

```bash
python3 /root/code/fraud-detection/src/models/bakeoff.py
```

Expected:

```text
Winner written to
/root/code/fraud-detection/reports/winner.json
```

<img width="767" height="70" alt="backoff" src="https://github.com/user-attachments/assets/55293554-f8c4-4171-a357-cbc2385bb5a9" />

# Verification

## Verify Winner File

```bash
cat /root/code/fraud-detection/reports/winner.json
```

Expected structure:

```json
{
  "model_type": "gradient_boosting",
  "run_id": "c2c73f5f4d0b4e0a9b1f5f2b8f93e1aa",
  "f1_score": 0.912345
}
```

*(actual values depend on the experiment results)*

<img width="732" height="140" alt="cli verify" src="https://github.com/user-attachments/assets/be5da49b-dc46-43d4-9f10-91f848f74f9f" />

## Verify Required Keys

Exactly:

```text
model_type
run_id
f1_score
```

No extra fields.

## Verify MLflow Experiment

Open:

```text
MLflow UI
→ bakeoff
```

Expected:

```text
3 Runs
```

One each for:

```text
random_forest
gradient_boosting
logistic_regression
```

## Verify Winner

Using Compare View:

Select all three runs.

Expected:

```text
Highest F1 Score
      ↓
winner.json
```

<img width="1887" height="437" alt="ui verify" src="https://github.com/user-attachments/assets/23a66902-6115-4337-93a1-46e10417616c" />

matches exactly.

# Fun Message 😄

*"A model bake-off is like a talent show for algorithms — only the one with the highest F1 score gets the trophy 🏆🤖"*
