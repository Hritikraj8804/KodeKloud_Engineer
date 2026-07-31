# Instruction

The xFusionCorp Industries ML Platform team wants every Pull Request to:

* Train the model
* Log the training run to MLflow
* Register the model automatically

The workflow already executes:

```bash
python3 -m src.register
```

However, `src/register.py` expects these environment variables:

* `MLFLOW_TRACKING_URI`
* `MLFLOW_TOKEN`

Since they aren't populated, the **register** job fails.

The correct solution is to:

1. Create **repository secrets**
2. Reference them inside the workflow using `${{ secrets.<NAME> }}`

# Solution

## Part 1 – Create Repository Secrets

Open Gitea:

```text
http://localhost:3000
```

Login:

```text
Username:
gitea-admin

Password:
gitea2026
```

Navigate:

```text
fraud-detector

↓

Settings

↓

Actions

↓

Secrets

↓

New Repository Secret
```

Create the following secrets:

| Secret Name           | Value                                                 |
| --------------------- | ----------------------------------------------------- |
| `MLFLOW_TRACKING_URI` | `http://localhost:5000`                               |
| `MLFLOW_TOKEN`        | `fraud-detector-ci-token` *(or any non-empty string)* |

## Part 2 – Update the Workflow

Modify the **register** job to expose the secrets through an `env:` block.

Example:

```yaml
register:
  runs-on: ubuntu-latest

  env:
    MLFLOW_TRACKING_URI: ${{ secrets.MLFLOW_TRACKING_URI }}
    MLFLOW_TOKEN: ${{ secrets.MLFLOW_TOKEN }}

  steps:
    - uses: actions/checkout@v4

    - name: Install registration deps
      run: |
        pip install --break-system-packages \
          mlflow numpy scikit-learn joblib pandas

    - name: Train and register
      run: python3 -m src.register
```

Using a step-level `env:` is also acceptable:

```yaml
- name: Train and register
  env:
    MLFLOW_TRACKING_URI: ${{ secrets.MLFLOW_TRACKING_URI }}
    MLFLOW_TOKEN: ${{ secrets.MLFLOW_TOKEN }}
  run: python3 -m src.register
```

Either approach satisfies the lab requirement.

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
* add-registry-push
```

## Step 2

Create repository secrets.

```
Settings
↓

Actions
↓

Secrets
```

Add:

```
MLFLOW_TRACKING_URI
```

Value:

```
http://localhost:5000
```

<img width="930" height="557" alt="tracking url" src="https://github.com/user-attachments/assets/99addb79-002f-4a85-9122-47e533084025" />

Add:

```
MLFLOW_TOKEN
```

Value:

```
fraud-detector-ci-token
```

<img width="912" height="527" alt="mlflow toaken" src="https://github.com/user-attachments/assets/9f5ff1c9-4e47-4a13-af68-6d8f1b2ad0f3" />

## Step 3

Edit the workflow.

```bash
vi .gitea/workflows/ci.yml
```

Update the `register` job:

```yaml
register:
  runs-on: ubuntu-latest

  env:
    MLFLOW_TRACKING_URI: ${{ secrets.MLFLOW_TRACKING_URI }}
    MLFLOW_TOKEN: ${{ secrets.MLFLOW_TOKEN }}

  steps:
    - uses: actions/checkout@v4

    - name: Install registration deps
      run: pip install --break-system-packages mlflow numpy scikit-learn joblib pandas

    - name: Train and register
      run: python3 -m src.register
```

## Step 4

Commit.

```bash
git add .gitea/workflows/ci.yml

git commit -m "Use repository secrets for MLflow registration"
```

## Step 5

Push.

```bash
git push
```

<img width="1132" height="225" alt="code push" src="https://github.com/user-attachments/assets/ced6b871-bdf9-49cb-89da-d514f16af05b" />

## Step 6

Open the Pull Request.

Wait for the workflow.

Expected:

```
✔ lint

✔ test

✔ register
```

<img width="985" height="601" alt="job" src="https://github.com/user-attachments/assets/05163705-bcd2-4e38-ac23-ce80b68c7766" />

## Step 7

Open MLflow.

```
http://localhost:5000
```

Navigate:

```
Models

↓

fraud-detector
```

Expected:

```
Version 1

(or newer)
```

indicating the model was successfully registered.

<img width="1627" height="706" alt="mlflow" src="https://github.com/user-attachments/assets/6499320a-5393-4b3e-9f49-8ab7fcda82b8" />

# Workflow

```text
Developer Push
       │
       ▼
 Gitea Actions
       │
       ▼
 Repository Secrets
       │
       ▼
Environment Variables
       │
       ▼
python3 -m src.register
       │
       ▼
 MLflow Tracking Server
       │
       ▼
 Model Registered
```

# Fun Message 😄

*"Great CI pipelines don't just automate ML—they automate it securely. Secrets stay hidden, workflows stay reusable, and models keep flowing into the registry! 🔐🚀"*
