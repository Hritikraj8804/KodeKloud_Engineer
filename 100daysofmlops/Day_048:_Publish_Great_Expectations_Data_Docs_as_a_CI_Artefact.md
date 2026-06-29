# Instruction

The xFusionCorp Industries ML Platform team integrated the **drift_check** Great Expectations checkpoint into the fraud-detector repository's CI pipeline.

The workflow successfully validates data quality on every Pull Request, but reviewers cannot inspect the generated **Data Docs** because they are deleted when the CI runner finishes.

The goal was to update the GitHub Actions/Gitea workflow so every CI run uploads the generated **Great Expectations Data Docs** as a downloadable workflow artifact.

# Solution

A final workflow step was added using **actions/upload-artifact**.

```yaml
- name: Upload Data Docs
  uses: actions/upload-artifact@v3
  with:
    name: data-docs
    path: gx/uncommitted/data_docs/
```

This archives the complete Data Docs directory as a downloadable artifact after every workflow run.

# Updated Workflow

```yaml
name: Data Quality

on:
  pull_request:
    branches: [main]

  push:
    branches: [main]

jobs:
  data-quality:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install Great Expectations
        run: |
          pip install --break-system-packages \
            great_expectations pandas numpy

      - name: Run drift_check checkpoint
        run: python3 -m src.gx_run

      - name: Upload Data Docs
        uses: actions/upload-artifact@v3
        with:
          name: data-docs
          path: gx/uncommitted/data_docs/
```

<img width="941" height="515" alt="upload data docs yaml" src="https://github.com/user-attachments/assets/cae2c362-602e-437c-bce0-c81475092567" />

# Solution Steps

## Step 1 - Configure Git Identity

```bash
git config user.name "Gitea Admin"
git config user.email "gitea-admin@example.com"
```

<img width="806" height="116" alt="set user" src="https://github.com/user-attachments/assets/5661be58-54eb-4af5-93bf-59973bfe4a1d" />

## Step 2 - Edit Workflow

Open:

```bash
vi .gitea/workflows/data-quality.yml
```

Append the upload artifact step.

## Step 3 - Commit Changes

```bash
git add .gitea/workflows/data-quality.yml

git commit -m "Publish Great Expectations Data Docs as workflow artifact"
```

<img width="792" height="47" alt="git add" src="https://github.com/user-attachments/assets/6f5869bb-0f32-4b84-839a-c901ce13536d" />

## Step 4 - Push the Branch

```bash
git push origin add-data-docs-artefact
```

<img width="795" height="280" alt="git push" src="https://github.com/user-attachments/assets/d6a6112f-3c85-476a-b557-4a4957196b49" />

This automatically triggered a fresh CI run for the Pull Request.

# Verification

## Workflow

Verify the workflow contains:

```yaml
uses: actions/upload-artifact@v3
```

Artifact name:

```text
data-docs
```

Artifact path:

```text
gx/uncommitted/data_docs/
```

## CI Status

The PR checks should complete successfully.

```text
✔ Data Quality
```

## Verify Artifact

After the workflow finishes:

Navigate to:

```text
Workflow Run
    ↓
Artifacts
```

Expected artifact:

```text
data-docs
```

<img width="1661" height="730" alt="gitea workflow" src="https://github.com/user-attachments/assets/d158530a-7f71-438c-9a1e-54f4c68ddcb5" />

## Download Artifact

Extract the ZIP archive.

Expected contents:

```text
index.html

validations/
    validation.html
    ...
```

The artifact contains the complete Great Expectations Data Docs website.


# Fun Message 😄

*"A green CI badge is nice—but a downloadable Data Docs report tells the whole story. Good MLOps isn't just about passing checks; it's about making the evidence easy to inspect! 📊🚀"*
