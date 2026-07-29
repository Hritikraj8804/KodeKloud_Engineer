# Instruction

The xFusionCorp Industries ML Platform team has three independent test suites:

* `test_train.py`
* `test_data_quality.py`
* `test_model_contract.py`

Currently, the CI workflow executes all tests in a single job:

```bash
python3 -m pytest tests -v
```

As the project grows, this becomes a bottleneck because all tests run serially.

Your task is to convert the **test** job into a **matrix strategy**, allowing each test suite to execute in its own parallel job.

# Solution

Replace the existing **test** job with:

```yaml
test:
  runs-on: ubuntu-latest

  strategy:
    matrix:
      suite:
        - train
        - data_quality
        - model_contract

  steps:
    - uses: actions/checkout@v4

    - name: Install pytest + runtime deps
      run: |
        pip install --break-system-packages \
          pytest pandas numpy scikit-learn joblib

    - name: Run test suite
      run: |
        python3 -m pytest tests/test_${{ matrix.suite }}.py -v
```

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
* add-test-matrix
```

## Step 2

Edit the workflow.

```bash
vi .gitea/workflows/ci.yml
```

Leave the **lint** job unchanged.

## Step 3

Replace:

```yaml
test:
  runs-on: ubuntu-latest

  steps:
    - uses: actions/checkout@v4

    - name: Install pytest + runtime deps
      run: pip install --break-system-packages pytest pandas numpy scikit-learn joblib

    - name: Run all tests
      run: python3 -m pytest tests -v
```

with:

```yaml
test:
  runs-on: ubuntu-latest

  strategy:
    matrix:
      suite:
        - train
        - data_quality
        - model_contract

  steps:
    - uses: actions/checkout@v4

    - name: Install pytest + runtime deps
      run: |
        pip install --break-system-packages \
        pytest pandas numpy scikit-learn joblib

    - name: Run test suite
      run: |
        python3 -m pytest tests/test_${{ matrix.suite }}.py -v
```

## Step 4

Commit.

```bash
git add .gitea/workflows/ci.yml

git commit -m "Convert test job to matrix strategy"
```

## Step 5

Push.

```bash
git push
```

<img width="732" height="392" alt="code push" src="https://github.com/user-attachments/assets/ec94f75f-5a15-4067-8aea-38ac467b55b7" />

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
Convert test job to matrix strategy
```

## Step 7

Open **Checks**.

Expected jobs:

```text
✔ lint

✔ test (train)

✔ test (data_quality)

✔ test (model_contract)
```

Merge once all checks succeed.

<img width="1237" height="892" alt="job runs" src="https://github.com/user-attachments/assets/40e47dbd-601f-44db-ab1a-c0c376d7d002" />

# Workflow

```text
Developer Push
      │
      ▼
 Gitea Actions
      │
      ▼
      Matrix
      │
 ┌────┼───────────────┐
 ▼    ▼               ▼
Train Data Quality Model Contract
 │        │              │
 ▼        ▼              ▼
PASS     PASS          PASS
      │
      ▼
Overall CI Success
```

# Fun Message 😄

*"Why wait for three test suites to stand in line? A matrix lets them all sprint to the finish together! 🏃‍♂️⚡"*
