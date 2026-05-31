## Instruction

The xFusionCorp Industries ML team needed to complete a production-ready DVC pipeline for the `fraud-detection` project.

The project already had:

* Git initialized ✅
* DVC initialized ✅
* SeaweedFS remote configured ✅
* Parameters defined in `params.yaml` ✅

However:

* One existing stage contained an incorrect output path
* Two pipeline stages were missing
* The full workflow needed to be reproduced, pushed to SeaweedFS, committed, and tagged as a release


# Solution

## ✅ Step 1 - Navigate to the Project

```bash
cd /root/code/ml-pipeline
```



# Step 2 - Copy Training Scripts

Copy the staged scripts:

```bash
cp scripts-staging/train.py scripts/

cp scripts-staging/evaluate.py scripts/
```

Verify:

```bash
ls scripts/
```

Expected:

```text
ingest.py
validate.py
preprocess.py
train.py
evaluate.py
```

<img width="722" height="138" alt="files" src="https://github.com/user-attachments/assets/cd25ca9f-632f-4627-a5b6-17ea699330b0" />


# Step 3 - Fix the Incorrect Preprocess Output

Inspect:

```bash
python scripts/preprocess.py
```

Check which file is actually created.

Update:

```yaml
preprocess:
```

to use the correct output path.

Example:

```yaml
outs:
  - data/processed/preprocessed.csv
```

if that is the file actually generated.



# Step 4 - Add the Train Stage

```yaml
train:
  cmd: python scripts/train.py
  deps:
    - data/processed/preprocessed.csv
    - scripts/train.py

  params:
    - n_estimators
    - max_depth
    - test_size
    - random_seed

  outs:
    - models/model.pkl
    - data/processed/test_split.csv

  metrics:
    - metrics.json:
        cache: false
```



# Step 5 - Add the Evaluate Stage

```yaml
evaluate:
  cmd: python scripts/evaluate.py
  deps:
    - models/model.pkl
    - data/processed/test_split.csv
    - scripts/evaluate.py

  metrics:
    - reports/evaluation.json:
        cache: false
```



# Final Pipeline Structure

```text
ingest
   ↓
validate
   ↓
preprocess
   ↓
train
   ↓
evaluate
```



# Step 6 - Reproduce the Full Pipeline

Run:

```bash
dvc repro
```

Expected execution:

```text
Running stage 'ingest'
Running stage 'validate'
Running stage 'preprocess'
Running stage 'train'
Running stage 'evaluate'
```

Generated outputs:

```text
models/model.pkl

metrics.json

reports/evaluation.json

data/processed/test_split.csv
```



# Step 7 - Verify Pipeline Status

```bash
dvc status
```

Expected:

```text
Data and pipelines are up to date.
```



# Step 8 - Push Artifacts to SeaweedFS

Upload DVC-tracked artifacts:

```bash
dvc push
```

Expected:

```text
Everything is up to date.
```

or

```text
X files pushed
```

<img width="781" height="591" alt="dvc" src="https://github.com/user-attachments/assets/b9a7deca-9fad-4f78-9fdd-8b738e8203f3" />


# Step 9 - Verify Remote Storage

Open SeaweedFS Filer.

Navigate to:

```text
/buckets/dvc-storage/files/md5/
```

You should see cached DVC objects stored remotely.



# Step 10 - Commit Everything

Stage files:

```bash
git add .
```

Commit:

```bash
git commit -m "Complete production DVC pipeline"
```

<img width="785" height="327" alt="git" src="https://github.com/user-attachments/assets/b12e696f-f9d9-4102-b886-906b2f94ed3b" />


# Step 11 - Create Release Tag

Tag the release:

```bash
git tag v1.0
```

Verify:

```bash
git tag
```

Expected:

```text
v1.0
```

# Fun Message 😄

*"A production ML pipeline isn't finished when the model trains—it's finished when anyone can reproduce it months later with one command 😄"*
