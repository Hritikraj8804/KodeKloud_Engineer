# Instruction

The xFusionCorp Industries ML Platform team added a **data-quality** CI job to validate the training data schema on every Pull Request.

The workflow is already configured with three jobs:

* ✅ Lint
* ✅ Test
* ❌ Data Quality (failing)

Your task is to:

1. Open the failed **Gitea Actions** run.
2. Read the **runtime log**.
3. Identify why the **data-quality** job failed.
4. Fix the workflow (without removing the job).
5. Commit and push the fix.
6. Ensure all three jobs pass.


# Solution

## Step 1 - Open the Failed Job

Open Gitea:

```text
http://localhost:3000
```

Login:

```text
Username: gitea-admin
Password: gitea2026
```

Navigate:

```text
fraud-detector
    ↓
Pull Requests
    ↓
Add data-quality CI gate
    ↓
Checks
    ↓
data-quality
```

Read the final error in the log.

## Step 2 - Find the Correct Test File

On the feature branch:

```bash
cd /root/code/fraud-detector

git branch
```

Verify you're on:

```text
add-data-validation
```

List the available test files:

```bash
ls tests
```

or

```bash
find tests -name "*.py"
```

Example output might be:

```text
test_train.py
test_schema.py
```

or

```text
test_dataset_validation.py
```

The important part is:

> Use **the filename that actually exists**.

## Step 3 - Update the Workflow

Open:

```bash
vi .gitea/workflows/ci.yml
```

Replace

```yaml
run: python3 -m pytest tests/test_data_validation.py -v
```

with the correct existing file, for example:

```yaml
run: python3 -m pytest tests/test_schema.py -v
```

**or**

```yaml
run: python3 -m pytest tests/test_dataset_validation.py -v
```

depending on what exists in the repository.

⚠️ Do **not** delete the `data-quality` job.

## Step 4 - Verify Locally

Run exactly the same command used in the workflow:

```bash
python3 -m pytest tests/<actual_test_file>.py -v
```

Expected:

```text
... passed
```

## Step 5 - Commit and Push

```bash
git add .gitea/workflows/ci.yml

git commit -m "Fix data-quality workflow test path"

git push
```

## Step 6 - Wait for CI

Refresh the Pull Request.

Expected:

```text
✔ lint

✔ test

✔ data-quality
```

<img width="1672" height="592" alt="runs" src="https://github.com/user-attachments/assets/71a4f6ca-4965-4f6b-8e21-ba40794b387c" />

<img width="947" height="870" alt="pr" src="https://github.com/user-attachments/assets/49c02ade-ab35-4f77-9c9b-17ae9c7fc732" />

All three jobs should become green.

# Workflow

```text
Developer Push
      │
      ▼
 Gitea Actions
      │
      ▼
  Lint Job ✔
      │
      ▼
  Test Job ✔
      │
      ▼
 Data-Quality Job
      │
      ▼
Read Failure Log
      │
      ▼
Fix Test Path
      │
      ▼
Push Changes
      │
      ▼
All Jobs Green ✔
```

# Fun Message 😄

*"A green pipeline isn't built by guessing—it's built by reading the logs, fixing the root cause, and letting automation prove the result! 🟢🔍"*
