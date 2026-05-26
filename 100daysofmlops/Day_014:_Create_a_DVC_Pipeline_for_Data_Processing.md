## Instruction

The xFusionCorp Industries ML team uses **DVC Pipelines** to automate and reproduce data processing workflows.

A draft `dvc.yaml` already existed inside the `fraud-detection` project, but:

```bash
dvc repro
```

failed to execute the complete pipeline.

The task was to inspect and correct the pipeline definition so that DVC could execute the entire workflow end-to-end.



# Requirements

The corrected pipeline needed to contain **two stages**:

## Stage 1: process_data

### Dependencies

```bash
data/raw/transactions.csv
src/data/process_data.py
```

### Output

```bash
data/processed/clean_transactions.csv
```



## Stage 2: split_data

### Dependencies

```bash
data/processed/clean_transactions.csv
src/data/split_data.py
```

### Outputs

```bash
data/processed/train.csv
data/processed/test.csv
```


# Solution

## Corrected dvc.yaml

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
```

<img width="877" height="532" alt="config" src="https://github.com/user-attachments/assets/182f9398-646f-4532-9df6-1c5d91f722f5" />


# Step-by-Step Execution

## 1. Navigate to the Project Directory

```bash
cd /root/code/fraud-detection
```



## 2. Edit dvc.yaml

```bash
vi dvc.yaml
```

or

```bash
nano dvc.yaml
```

Replace the contents with the corrected pipeline.



## 3. Reproduce the Pipeline

Run:

```bash
dvc repro
```

Expected execution flow:

```text
process_data
      ↓
split_data
```

DVC automatically executes stages in dependency order.

<img width="682" height="442" alt="repro" src="https://github.com/user-attachments/assets/7cedd59b-98b3-4b36-b5b3-caf27b963d13" />


## 4. Verify Pipeline Status

Run:

```bash
dvc status
```

Expected output:

```text
Data and pipelines are up to date.
```

or

```text
Pipeline is up to date.
```

No stale stages should be reported.

<img width="640" height="72" alt="status" src="https://github.com/user-attachments/assets/50d1e648-d627-487b-9a36-2902f770a43a" />


## 5. Visualize the Pipeline

Optional:

```bash
dvc dag
```

Expected graph:

```text
process_data
      ↓
split_data
```

The VS Code DVC extension will also display this relationship in the **PIPELINES** section.



# Understanding the Concept 💡

## What is a DVC Pipeline?

A DVC Pipeline defines:

* Data dependencies
* Processing steps
* Outputs

allowing ML workflows to be reproduced automatically.



## Why Use Dependencies?

Dependencies tell DVC:

> "If this file changes, rerun the stage."

Example:

```yaml
deps:
  - data/raw/transactions.csv
```

If the dataset changes, DVC automatically reruns downstream stages.



## Why Define Outputs?

Outputs tell DVC:

> "This stage produces these files."

Example:

```yaml
outs:
  - data/processed/train.csv
```

DVC tracks and versions these outputs.



# Challenges Faced 🚧

* Incorrect script references
* Wrong output file names
* Missing stage dependencies
* Broken pipeline graph



# Key Learnings 📚

* Learned how DVC pipelines work
* Practiced defining dependencies and outputs
* Understood stage chaining
* Explored reproducible ML workflows
* Learned how DVC tracks data lineage



# Commands Used 🖥️

```bash
cd /root/code/fraud-detection

vi dvc.yaml

dvc repro

dvc status

dvc dag
```



# Fun Message 😄

*"DVC pipelines are basically workflow automation for data scientists who got tired of remembering which script to run next 😄"*

