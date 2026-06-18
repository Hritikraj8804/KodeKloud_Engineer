# Instruction

The xFusionCorp Industries ML Platform team runs fraud detection training through a four-stage pipeline:

```text
Raw Data
    ↓
Preprocess
    ↓
Featurize
    ↓
Train
    ↓
Evaluate
```

The entire workflow is orchestrated by:

```bash
run_pipeline.py
```

and tracked as a single MLflow run.

However, the pipeline violated a critical stage-chain invariant:

> Every stage must consume the output of the previous stage.

The **preprocess** stage correctly removed:

* Rows where `amount < 50`
* Duplicate records

and saved the cleaned dataset.

Unfortunately, the **featurize** stage ignored that cleaned dataset and started from the raw CSV again, causing downstream stages to train on data that had never been cleaned.

# Solution Steps

## Step 1 - Edit File

```bash
vi /root/code/fraud-detection/src/featurize.py
```

Change:

```python
config["data"]["raw_path"]
```

to:

```python
config["data"]["processed_path"]
```

Save the file.

## Step 2 - Run Pipeline

From project root:

```bash
cd /root/code/fraud-detection

python3 run_pipeline.py
```

Expected:

```text
[preprocess] ...
[featurize] ...
[train] ...
[evaluate] ...
```

<img width="727" height="257" alt="run" src="https://github.com/user-attachments/assets/50de2221-818a-468e-bfc9-a96684bdf336" />

Pipeline completes successfully.

# Verification

## Verify Row Counts

Raw dataset:

```bash
wc -l data/raw/train.csv
```

Expected:

```text
201
```

(200 rows + header)

Processed dataset:

```bash
wc -l data/processed/train_clean.csv
```

Expected:

```text
< 201
```

because rows were removed.

Features dataset:

```bash
wc -l data/features/features.csv
```

Expected:

```text
same as train_clean.csv
```

The counts must match exactly.

## Verify Model

```bash
ls -l models/model.pkl
```

Expected:

```text
model.pkl
```

exists.

## Verify Evaluation Report

```bash
cat reports/evaluation.json
```

Expected structure:

```json
{
  "accuracy": 0.91,
  "f1": 0.88,
  "roc_auc": 0.94
}
```

(values vary)

<img width="695" height="86" alt="verify" src="https://github.com/user-attachments/assets/7063e151-12c7-4f1f-a536-ee4e799110fc" />

## Verify MLflow

Open:

```text
MLflow UI
→ training-pipeline
```

Expected:

### One Run

```text
1 run
```

### Logged Parameters

```text
model_type
n_estimators
max_depth
```

### Logged Metrics

```text
accuracy
f1
roc_auc
```

<img width="1817" height="861" alt="ui" src="https://github.com/user-attachments/assets/bf503ff3-d7a0-4738-b194-030a478b8b6f" />

# Fun Message 😄

*"A pipeline is only as trustworthy as its weakest stage connection. One wrong input path can undo every cleaning step before it 😄🔗"*
