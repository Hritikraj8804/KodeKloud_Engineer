# Instruction

The xFusionCorp Industries ML Platform team wants to enforce **ML quality gates** at two levels:

1. **Pre-deployment (CI/CD)** using an **Evidently Test Suite**.
2. **Production monitoring** using a **Grafana alert**.

The objectives are:

* Complete the `METRICS` TODO in `test_suite.py`
* Run the test suite successfully
* Publish the results to the Evidently UI
* Create a Grafana alert that fires when the rolling prediction accuracy drops below **0.80**

# Solution

## Part 1 — Complete `test_suite.py`

Inside the `METRICS` list, add the two thresholded tests:

```python
from evidently.tests import TestNumberOfMissingValues
from evidently.tests import TestAccuracyScore

METRICS = [
    TestNumberOfMissingValues(lt=10),
    TestAccuracyScore(gt=0.80),
]
```

## Run the Test Suite

```bash
cd /root/code/monitoring/tests

python3 test_suite.py
```

Expected:

```text
SUCCESS
SUCCESS
```

<img width="741" height="217" alt="exec test" src="https://github.com/user-attachments/assets/5a91ebb2-2f7d-4096-a2ec-93cf3d033241" />

A new file should be generated:

```text
test_results.json
```

<img width="731" height="110" alt="result" src="https://github.com/user-attachments/assets/9e5703ed-b44e-40a2-8e5d-30ff694d3975" />

The run is also published to the Evidently UI.

## Part 2 — Create Grafana Alert

Navigate:

```text
Alerts
    ↓
Alert Rules
    ↓
New Alert Rule
```

Datasource:

```text
Prometheus
```

PromQL expression:

```promql
avg_over_time(prediction_accuracy[1m])
```

Condition:

```text
IS BELOW

0.80
```

Evaluation:

```text
Every: 1 minute
```

Save the rule.

Example name:

```text
Prediction Accuracy Below 80%
```

<img width="1047" height="871" alt="alert ui" src="https://github.com/user-attachments/assets/692fba6c-2897-4019-b52c-d47438d75a48" />

# Solution Steps

## Step 1

Navigate:

```bash
cd /root/code/monitoring/tests
```

## Step 2

Edit:

```bash
vi test_suite.py
```

Insert:

```python
TestNumberOfMissingValues(lt=10)

TestAccuracyScore(gt=0.80)
```

## Step 3

Run:

```bash
python3 test_suite.py
```

Expected:

```text
SUCCESS

SUCCESS
```

## Step 4

Verify:

```bash
ls
```

Expected:

```text
test_results.json
```

## Step 5

Open Evidently UI

```text
http://localhost:8000
```

Navigate to:

```text
fraud-detector

↓

Reports
```

<img width="1872" height="622" alt="evidently ui" src="https://github.com/user-attachments/assets/1279bf31-6446-442e-b0f3-50e6dc026026" />

Verify that a new snapshot appears.

## Step 6

Open Grafana

```text
http://localhost:3000
```

Navigate:

```text
Alerts

↓

Alert Rules

↓

New Rule
```

Datasource:

```text
Prometheus
```

Query:

```promql
avg_over_time(prediction_accuracy[1m])
```

Condition:

```text
Below

0.80
```

Save.

# Verification

## Verify Test Results

```bash
cat /root/code/monitoring/tests/test_results.json
```

Expected:

```json
{
  "tests":[
    {
      "status":"SUCCESS"
    },
    {
      "status":"SUCCESS"
    }
  ]
}
```

## Verify Snapshot

Open:

```text
http://localhost:8000
```

Project:

```text
fraud-detector
```

Reports tab should contain the newly published run.

## Verify Alert Rules

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/v1/provisioning/alert-rules
```

Expected:

* Non-empty array
* Contains at least one rule

## Verify Rule

The alert should contain:

```promql
avg_over_time(prediction_accuracy[1m])
```

Threshold:

```text
0.80
```

# Workflow

```text
             Evaluation Batch
                    │
                    ▼
        Evidently Test Suite
      ┌─────────────┴─────────────┐
      │                           │
Missing Values Test        Accuracy Test
      │                           │
      └─────────────┬─────────────┘
                    ▼
             test_results.json
                    │
                    ▼
          Publish to Evidently UI

────────────────────────────────────

Live Production Metrics
        │
        ▼
prediction_accuracy
        │
        ▼
Prometheus
        │
        ▼
Grafana Alert Rule
avg_over_time(prediction_accuracy[1m])
        │
        ▼
Alert if < 0.80
```

# Fun Message 😄

*"The best ML systems are protected twice: once before deployment with quality gates, and again in production with real-time alerts. Prevention and detection make the perfect team! 🛡️📈"*
