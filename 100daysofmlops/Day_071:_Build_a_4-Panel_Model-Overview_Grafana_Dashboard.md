# Day 073 - Building a Model Overview Dashboard in Grafana 📊🚀

## Instruction

The xFusionCorp Industries ML Platform team needs a **Model Overview Dashboard** that gives on-call engineers a quick snapshot of the fraud-detection model's health.

The dashboard must include the following four metrics:

* **Request Rate**
* **p95 Inference Latency**
* **Prediction Accuracy**
* **Per-Feature Data Drift (PSI)**

Additionally, the dashboard should use **at least three different visualization types**.


# Solution

## Step 1 - Open Grafana

Open:

```text
http://localhost:3000
```

Login:

```text
Username: admin
Password: grafana2026
```

## Step 2 - Create Dashboard

Navigate:

```text
Dashboards
    ↓
New
    ↓
New Dashboard
```

Click:

```text
Add Visualization
```

Select the **Prometheus** datasource.

# Panel 1 — Request Rate

### Visualization

```text
Time Series
```

### PromQL

```promql
sum(rate(flask_http_request_total[1m]))
```

This shows the total requests per second over the last minute.

# Panel 2 — p95 Inference Latency

### Visualization

```text
Stat
```

### PromQL

```promql
histogram_quantile(
  0.95,
  sum(rate(model_inference_duration_seconds_bucket[5m])) by (le)
)
```

Displays the 95th percentile inference latency.

# Panel 3 — Prediction Accuracy

### Visualization

```text
Gauge
```

### PromQL

```promql
prediction_accuracy
```

Shows the current rolling prediction accuracy.

# Panel 4 — Drift by Feature

### Visualization

```text
Bar Gauge
```

### PromQL

```promql
data_drift_score
```

Each feature becomes an individual bar:

* amount
* hour
* num_tx_past_day

## Step 3 - Arrange Dashboard

Example layout:

```text
+--------------------+--------------------+
| Request Rate       | p95 Latency        |
| (Time Series)      | (Stat)             |
+--------------------+--------------------+
| Prediction Accuracy| Drift by Feature   |
| (Gauge)            | (Bar Gauge)        |
+--------------------+--------------------+
```

## Step 4 - Save Dashboard

Example name:

```text
Fraud Detection Model Overview
```

# Solution Steps

## Step 1

Open:

```text
http://localhost:3000
```

Login:

```text
admin

grafana2026
```

## Step 2

Create:

```text
Dashboards

↓

New Dashboard
```

## Step 3

Create the four panels.

### Panel 1

Visualization

```text
Time Series
```

Query

```promql
sum(rate(flask_http_request_total[1m]))
```

### Panel 2

Visualization

```text
Stat
```

Query

```promql
histogram_quantile(
0.95,
sum(rate(model_inference_duration_seconds_bucket[5m])) by (le)
)
```

### Panel 3

Visualization

```text
Gauge
```

Query

```promql
prediction_accuracy
```

### Panel 4

Visualization

```text
Bar Gauge
```

Query

```promql
data_drift_score
```

## Step 4

Save the dashboard.

# Verification

## Verify Dashboard Exists

```bash
curl -u admin:grafana2026 \
"http://localhost:3000/api/search?type=dash-db"
```

Expected:

<img width="727" height="177" alt="grafana-db-cli" src="https://github.com/user-attachments/assets/58d8a2d4-3f1a-454e-9a26-d954cd9a29ab" />

At least one dashboard.

## Verify Dashboard JSON

Retrieve dashboard UID:

```bash
curl -u admin:grafana2026 \
"http://localhost:3000/api/search?type=dash-db"
```

Then inspect:

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/dashboards/uid/<dashboard_uid>
```

<img width="717" height="210" alt="grafana-dbs-cli" src="https://github.com/user-attachments/assets/230e6581-dfe8-49c2-ae67-c324cb5a4c93" />

Expected dashboard contains:

### Metric 1

```promql
flask_http_request_total
```

### Metric 2

```promql
model_inference_duration_seconds_bucket
```

### Metric 3

```promql
prediction_accuracy
```

### Metric 4

```promql
data_drift_score
```

### Visualization Types

Expected panel types include at least three distinct types, for example:

```text
timeseries
stat
gauge
bargauge
```

<img width="1906" height="827" alt="dashboard-ui" src="https://github.com/user-attachments/assets/7d377310-2722-4b8c-9598-31dd9f39391f" />

# Workflow

```text
                Metric Emitter
                      │
                      ▼
                 Prometheus
                      │
                      ▼
        ┌──────────────────────────┐
        │       Grafana            │
        │                          │
        │  Time Series → Requests  │
        │  Stat → p95 Latency      │
        │  Gauge → Accuracy        │
        │  Bar Gauge → Drift       │
        └──────────────────────────┘
                      │
                      ▼
          Model Overview Dashboard
```

# Fun Message 😄

*"A great model dashboard is like an aircraft cockpit—traffic, latency, accuracy, and drift are all visible at a glance, so you can spot trouble before it becomes an incident! ✈️📊"*
