# Instruction

The xFusionCorp Industries ML Platform team wants every Pull Request to automatically run:

* **Ruff** (linting)
* **Pytest** (unit testing)

before code review.

A workflow template already exists at:

```text
.gitea/workflows/ci.yml.template
```

Your task is to:

1. Rename it to `ci.yml`
2. Complete the two missing `run:` commands
3. Commit and push the changes on the `add-ci` branch
4. Open a Pull Request
5. Wait for the CI workflow to pass
6. Merge the Pull Request into `main`

# Solution Steps

## Step 1

Create the feature branch.

```bash
cd /root/code/fraud-detector

git checkout -b add-ci
```

## Step 2

Rename the workflow.

```bash
mv .gitea/workflows/ci.yml.template \
   .gitea/workflows/ci.yml
```

## Step 3

Edit:

```bash
vi .gitea/workflows/ci.yml
```

Fill in:

```yaml
run: ruff check src tests
```

and

```yaml
run: pytest -v tests/
```

## Step 4

Verify locally.

Run Ruff:

```bash
ruff check src tests
```

Expected:

```text
All checks passed!
```

Run tests:

```bash
pytest -v tests/
```

Expected:

```text
3 passed
```

## Step 5

Commit.

```bash
git add .gitea/workflows/ci.yml

git commit -m "Add Gitea Actions CI workflow"
```

## Step 6

Push.

```bash
git push origin add-ci
```

<img width="777" height="397" alt="git push" src="https://github.com/user-attachments/assets/fd0f5708-3c5f-4023-b038-e94accf89a63" />

## Step 7

Open Gitea

```text
http://localhost:3000
```

Login:

```text
gitea-admin

gitea2026
```

Open repository:

```text
fraud-detector
```

Create a Pull Request:

```text
main ← add-ci
```

## Step 8

Open the **Checks** tab.

Expected:

```text
✓ lint

✓ test
```

<img width="1290" height="602" alt="wrokflow" src="https://github.com/user-attachments/assets/ff217022-fdbc-4298-b603-6c57b541e635" />

Both should complete successfully.

## Step 9

Merge the Pull Request into `main`.

<img width="971" height="795" alt="merge" src="https://github.com/user-attachments/assets/f49d00e5-cf02-4745-8cb1-ff1a18f9ed04" />

# Workflow

```text
Developer Pushes Feature Branch
              │
              ▼
        Pull Request Created
              │
              ▼
      Gitea Actions Workflow
      ┌─────────────────────┐
      │  Ruff Lint Job      │
      │  Pytest Job         │
      └─────────────────────┘
              │
              ▼
        Checks Pass ✓
              │
              ▼
      Merge into main
```

# Fun Message 😄

*"The best pull requests arrive with proof—they don't just say the code works, they let CI demonstrate it automatically! 🤖✅"*
