# Instruction

The xFusionCorp Industries ML Platform team completed their first **end-to-end production release** for the fraud-detection platform.

The release pipeline integrated four core MLOps components:

* 🔐 HashiCorp Vault (Secrets Management)
* 📊 Great Expectations (Data Quality)
* 🌿 Gitea Actions (CI/CD)
* 🤖 MLflow Model Registry (Model Promotion)

The workflow was already implemented in `.gitea/workflows/production.yml`.

The objective was to drive the entire release process using the respective web interfaces.


# Solution

## Step 1 - Stage the Vault Secret

Open the **Vault UI**.

Login using the token stored in:

```bash
cat /root/code/vault-token
```

Navigate to:

```text
secret/
```

Create:

```text
Path:
mlflow
```

Add the key:

```text
mlflow_password
```

Value:

```text
any non-empty value
```

Example:

```text
mlflow_password = xfusion123
```

Save the secret.

<img width="1097" height="677" alt="create mlflow screte valut" src="https://github.com/user-attachments/assets/92469918-53fd-46af-b34d-835a75791efa" />

## Step 2 - Open Pull Request

Open the **Gitea UI**.

Repository:

```text
fraud-detector
```

Create a Pull Request:

```text
production-release
        ↓
main
```

Submit the PR.

<img width="1596" height="841" alt="pull rq prod to main" src="https://github.com/user-attachments/assets/3b9278a3-2308-429c-8b21-3a54102449d1" />

## Step 3 - Wait for Workflow

The workflow automatically starts.

Pipeline:

```text
fetch-secret
        ↓
data-quality
        ↓
register-model
```

All three jobs pass successfully.

## Step 4 - Merge Pull Request

Once every check is green:

Click:

```text
Merge Pull Request
```

The release is now merged into **main**.

<img width="1455" height="817" alt="merge pull rq" src="https://github.com/user-attachments/assets/47c8639c-336f-4990-a046-906ede67c15f" />

## Step 5 - Verify Data Docs

Open:

```text
Data Docs
```

Verify the latest validation report.

Expected:

```text
schema_check

Success
```

Every expectation should be green.

## Step 6 - Verify MLflow

Open:

```text
Models
```

Expected registered model:

```text
fraud-detector
```

Open the model.

Assign:

```text
Alias

production
```

to the latest version.

<img width="1507" height="540" alt="mlflow" src="https://github.com/user-attachments/assets/160cc5bc-f691-4391-84eb-3ba54ef7ffa5" />

# Verification

## Vault

Secret exists:

```text
secret/mlflow
```

Contains:

```text
mlflow_password
```

## Pull Request

Merged:

```text
production-release
        ↓
main
```

## CI Workflow

All jobs:

```text
fetch-secret      ✅

data-quality      ✅

register-model    ✅
```

## Data Docs

Latest report:

```text
schema_check

Success
```

## MLflow

Registered model:

```text
fraud-detector
```

Alias:

```text
production
```

# Fun Message 😄

*"Fifty days ago we trained our first model. Today we shipped an end-to-end production MLOps pipeline with automated quality gates, secure secrets, CI/CD, and model governance. That's not just training models—that's delivering machine learning like software! 🚀🤖"*
