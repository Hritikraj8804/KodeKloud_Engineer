# Instruction

The xFusionCorp Industries ML Platform team wants the training outputs generated during CI to remain available after the workflow finishes.

Currently, the **report** job successfully generates:

* `artifacts/model.joblib`
* `artifacts/metrics.json`
* `artifacts/confusion_matrix.png`

However, these files are deleted when the runner is destroyed.

Your task is to use **`actions/upload-artifact@v3`** so reviewers can download the generated artifacts directly from the workflow run.

> **Note:** Use **`actions/upload-artifact@v3`**, as Gitea Actions does **not** support `@v4`.

# Solution Steps

## Step 1

Open the repository.

```bash
cd /root/code/fraud-detector
```

Verify branch:

```bash
git branch
```

Expected:

```text
* add-artifact-upload
```

## Step 2

Edit the workflow.

```bash
vi .gitea/workflows/ci.yml
```

## Step 3

Add the upload step:

```yaml
- name: Upload training artifacts
  uses: actions/upload-artifact@v3
  with:
    name: model-report
    path: artifacts/
```

## Step 4

Commit the changes.

```bash
git add .gitea/workflows/ci.yml

git commit -m "Upload training artifacts from CI"
```

## Step 5

Push the branch.

```bash
git push
```

<img width="731" height="377" alt="push" src="https://github.com/user-attachments/assets/9d5dc09b-ee51-441b-89b3-be6563822405" />

## Step 6

Open Gitea.

```text
http://localhost:3000
```

Login:

```text
Username: gitea-admin

Password: gitea2026
```

Open the Pull Request:

```text
Publish Training Artifacts from CI
```

## Step 7

Wait for the workflow to complete.

Expected jobs:

```text
✔ lint

✔ test

✔ report
```

<img width="1056" height="681" alt="job" src="https://github.com/user-attachments/assets/7eb475d2-234f-47ba-b7b4-13d647c56677" />

## Step 8

Open the workflow run.

Navigate to:

```text
Artifacts
```

Expected artifact:

```text
model-report
```

Download it.

The ZIP should contain:

```text
metrics.json

confusion_matrix.png

model.joblib
```

(The lab specifically requires at least `metrics.json` and `confusion_matrix.png`.)

<img width="987" height="277" alt="image" src="https://github.com/user-attachments/assets/dc75e973-e609-4af8-9eb9-e8987f10698c" />

# Workflow

```text
Checkout
    │
    ▼
Install Dependencies
    │
    ▼
Train Model
    │
    ▼
Generate Metrics
    │
    ▼
Create Confusion Matrix
    │
    ▼
Upload artifacts/
    │
    ▼
model-report.zip
    │
    ▼
Download from Run Page
```

# Fun Message 😄

*"A green CI check says the pipeline succeeded—but an uploaded artifact lets everyone see exactly what the pipeline produced. That's where ML reviews become evidence-based! 📦📈"*
