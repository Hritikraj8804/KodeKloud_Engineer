## Instruction

The xFusionCorp Industries Data Science team trained three different machine learning models and logged them to the MLflow experiment:

```text
model-comparison
```

The experiment contained three runs:

* RandomForest
* GradientBoosting
* LogisticRegression

Each run included:

* Accuracy
* F1 Score

The objective was to compare all three runs and explicitly mark the best-performing model so downstream automation could identify the production candidate.



# Requirements

## Selection Rule

Choose the run with the highest:

```text
metrics.f1_score
```



## Winning Run

Add a run-level tag:

| Key                  | Value |
| -------------------- | ----- |
| production-candidate | true  |



## Remaining Runs

The other two runs must:

```text
NOT have a production-candidate tag
```



# Solution

## ✅ Step 1 - Open MLflow UI

Open:

```text
MLflow UI
```

Navigate to:

```text
model-comparison
```

experiment.



## Step 2 - Compare Runs Side-by-Side

Select:

```text
RandomForest
GradientBoosting
LogisticRegression
```

Enable comparison view and inspect:

```text
accuracy
f1_score
```

Example:

| Model              | Accuracy | F1 Score |
| ------------------ | -------- | -------- |
| RandomForest       | 0.91     | 0.89     |
| GradientBoosting   | 0.94     | 0.92 ✅   |
| LogisticRegression | 0.88     | 0.86     |

*(Actual values depend on the lab.)*



## Step 3 - Identify the Winner

Locate the highest:

```text
f1_score
```

Example:

```text
GradientBoosting
```



## Step 4 - Add Production Candidate Tag

Open the winning run.

Add tag:

```text
production-candidate = true
```

Save.

<img width="1490" height="442" alt="tag" src="https://github.com/user-attachments/assets/419dcee5-be88-45f7-a296-52c20f1706d3" />

## Step 5 - Verify Other Runs

Open the remaining two runs.

Ensure:

```text
production-candidate
```

does not exist.

If present, remove it.



# CLI Verification 🔍

You can verify the tag using MLflow's backend database or MLflow CLI.

## Option 1 - Verify via MLflow UI

Open:

```text
model-comparison
```

Only one run should show:

```text
production-candidate = true
```



## Option 2 - Verify Using SQLite Backend

If MLflow uses SQLite:

```bash
sqlite3 /root/code/mlflow-backend/mlflow.db
```

List tagged runs:

```sql
SELECT run_uuid, key, value
FROM tags
WHERE key='production-candidate';
```

Expected:

```text
<single_run_id>|production-candidate|true
```

Only one record should be returned.

Exit:

```sql
.quit
```



## Option 3 - Verify Using Python

```bash
python3
```

```python
import mlflow

client = mlflow.tracking.MlflowClient(
    tracking_uri="http://localhost:5000"
)

exp = client.get_experiment_by_name("model-comparison")

runs = mlflow.search_runs(
    experiment_ids=[exp.experiment_id]
)

print(
    runs[["tags.production-candidate",
          "metrics.f1_score",
          "tags.mlflow.runName"]]
)
```

Expected:

```text
production-candidate=true
```

for exactly one run.

<img width="537" height="430" alt="verify" src="https://github.com/user-attachments/assets/3bd4bc4f-82c0-4f0f-a483-89ea0258c7e8" />

# Fun Message 😄

*"Three models enter the arena, one gets the production-candidate tag. MLflow's version of a talent show 😄🏆"*

