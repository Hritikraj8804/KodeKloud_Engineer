## Instruction

After training a model, the xFusionCorp Industries ML team wanted DVC to automatically surface model evaluation metrics through:

* `dvc metrics show`
* VS Code DVC Extension → **METRICS** section

The project already generated:

```bash
models/model.pkl
```

and

```bash
metrics.json
```

during the training stage.

However, DVC was treating `metrics.json` as a normal output file instead of a metric.

The task was to properly register the metrics file with DVC.



# Requirements

The existing training script:

```bash
src/models/train.py
```

already generated:

| File             | Purpose                  |
| ---------------- | ------------------------ |
| models/model.pkl | Trained model            |
| metrics.json     | Model evaluation metrics |

The `metrics.json` file needed to be:

* Registered as a DVC metric
* Stored with:

```yaml
cache: false
```

* Visible through:

```bash
dvc metrics show
```



# Existing dvc.yaml

```yaml
stages:
  process_data:
    cmd: python src/data/process_data.py
    deps:
      - data/raw/transactions.csv
      - src/data/process_data.py
    outs:
      - data/processed/clean_transactions.csv

  split_data:
    cmd: python src/data/split_data.py
    deps:
      - data/processed/clean_transactions.csv
      - src/data/split_data.py
    outs:
      - data/processed/train.csv
      - data/processed/test.csv

  train:
    cmd: python src/models/train.py
    deps:
      - data/processed/train.csv
      - src/models/train.py
    outs:
      - models/model.pkl
      - metrics.json
```


# Solution

## Corrected Train Stage

Replace:

```yaml
outs:
  - models/model.pkl
  - metrics.json
```

with:

```yaml
outs:
  - models/model.pkl

metrics:
  - metrics.json:
      cache: false
```



# Final dvc.yaml

```yaml
stages:
  process_data:
    cmd: python src/data/process_data.py
    deps:
      - data/raw/transactions.csv
      - src/data/process_data.py
    outs:
      - data/processed/clean_transactions.csv

  split_data:
    cmd: python src/data/split_data.py
    deps:
      - data/processed/clean_transactions.csv
      - src/data/split_data.py
    outs:
      - data/processed/train.csv
      - data/processed/test.csv

  train:
    cmd: python src/models/train.py
    deps:
      - data/processed/train.csv
      - src/models/train.py
    outs:
      - models/model.pkl

    metrics:
      - metrics.json:
          cache: false
```

<img width="657" height="627" alt="dvcpipeline" src="https://github.com/user-attachments/assets/8ca2a110-92a9-4353-bf2f-936df5585e9e" />


# Step-by-Step Execution

## 1. Navigate to the Project

```bash
cd /root/code/fraud-detection
```



## 2. Edit dvc.yaml

```bash
vi dvc.yaml
```

Update the train stage.



## 3. Reproduce the Pipeline

Run:

```bash
dvc repro
```

Expected execution:

```text
process_data
     ↓
split_data
     ↓
train
```

or only the necessary stages if outputs already exist.

<img width="612" height="377" alt="repro" src="https://github.com/user-attachments/assets/039ee3c8-35c0-42bc-b62d-dbda3e83af0d" />


## 4. Verify Metrics Registration

Run:

```bash
dvc metrics show
```

Expected output:

```text
Path          accuracy    f1_score
metrics.json  0.95        0.93
```

(Actual values depend on the training script.)

<img width="615" height="73" alt="metrics" src="https://github.com/user-attachments/assets/00daef42-1c7c-4a44-97b0-96e5e2f2b3eb" />


## 5. Verify dvc.lock

Inspect:

```bash
cat dvc.lock
```

You should see metrics tracked under the train stage.



# Understanding the Concept 💡

## What Are DVC Metrics?

Metrics are files that store evaluation results such as:

```json
{
  "accuracy": 0.95,
  "f1_score": 0.93
}
```

DVC can automatically compare these across experiments.



## Why Use `cache: false`?

For metrics:

```yaml
cache: false
```

means:

* File remains in Git
* Easy to review in pull requests
* Visible in Git diffs
* Not stored in DVC cache

Perfect for small JSON metric files.



## Difference Between Outputs and Metrics

| Type    | Purpose                                |
| ------- | -------------------------------------- |
| outs    | Models, datasets, artifacts            |
| metrics | Accuracy, F1 score, evaluation results |

Example:

```yaml
outs:
  - models/model.pkl

metrics:
  - metrics.json:
      cache: false
```



# Challenges Faced 🚧

* Metrics registered as normal outputs
* Understanding DVC metric tracking
* Learning when to use `cache: false`
* Making metrics visible to DVC tooling



# Key Learnings 📚

* Learned how DVC tracks evaluation metrics
* Practiced configuring metric outputs
* Understood `cache: false`
* Explored experiment monitoring workflows
* Learned how DVC integrates metrics into reproducible ML pipelines



# Commands Used 🖥️

```bash
cd /root/code/fraud-detection

vi dvc.yaml

dvc repro

dvc metrics show

cat dvc.lock
```



# Fun Message 😄

*"A model without metrics is just a guess with extra steps 😄📈"*

