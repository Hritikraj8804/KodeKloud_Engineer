# Instruction

The xFusionCorp Industries ML Platform team wanted to eliminate duplicated workflow logic in their CI pipeline.

The repository already contains three reusable workflows:

* `lint.yml`
* `test.yml`
* `report.yml`

Each reusable workflow is configured with:

```yaml
on:
  workflow_call:
```

The `main.yml` workflow already calls `lint.yml` using `uses:`, but the **test** and **report** jobs were still duplicated inline.

The objective was to convert the remaining two jobs into reusable workflow calls.


# Solution Steps

## Step 1 - Navigate to Repository

```bash
cd /root/code/fraud-detector
```

## Step 2 - Verify Branch

```bash
git branch
```

Expected:

```text
* add-reusable-workflows
```

## Step 3 - Edit Workflow

```bash
vi .gitea/workflows/main.yml
```

Replace the inline **test** job:

```yaml
test:
  runs-on: ubuntu-latest
  steps:
    ...
```

with:

```yaml
test:
  uses: ./.gitea/workflows/test.yml
```

Replace the inline **report** job:

```yaml
report:
  runs-on: ubuntu-latest
  steps:
    ...
```

with:

```yaml
report:
  uses: ./.gitea/workflows/report.yml
```

<img width="596" height="525" alt="workflow" src="https://github.com/user-attachments/assets/9430ef6b-24fe-4281-8d6b-dfdeb7c74acf" />

## Step 4 - Commit Changes

```bash
git add .gitea/workflows/main.yml

git commit -m "Refactor CI to reusable workflows"
```

## Step 5 - Push Changes

```bash
git push
```

<img width="735" height="305" alt="code push" src="https://github.com/user-attachments/assets/1232ded9-fed3-4ab0-8a33-81770dc03329" />

## Step 6 - Open Gitea

```text
http://localhost:3000
```

Login:

```text
Username: gitea-admin

Password: gitea2026
```

Open the PR:

```text
Convert test job to reusable workflows
```

Wait for the workflow to complete.

Expected:

```text
✔ lint

✔ test

✔ report
```

<img width="992" height="587" alt="jobs" src="https://github.com/user-attachments/assets/4e64904b-f83e-42ff-8722-297eac957c29" />

<img width="731" height="812" alt="pr" src="https://github.com/user-attachments/assets/f3483592-fb00-4d32-876f-0b798650a60e" />

# Workflow

```text
Pull Request
      │
      ▼
main.yml
      │
      ├──────────────┐
      │              │
      ▼              ▼
lint.yml         test.yml
      │              │
      └──────┐  ┌────┘
             ▼  ▼
         report.yml
             │
             ▼
       CI Completed
```

# Fun Message 😄

*"Why copy the same CI job three times when you can teach one workflow to do it and let everyone else call it? Reusable workflows keep your pipelines DRY and your maintenance easy! 🔄🚀"*
