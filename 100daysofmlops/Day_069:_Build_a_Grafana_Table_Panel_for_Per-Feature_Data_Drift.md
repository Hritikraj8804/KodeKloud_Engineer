# Instruction

The xFusionCorp Industries ML Platform team monitors **per-feature data drift** for their fraud-detection model using a **Grafana Table**.

The monitoring stack is already configured:

* ✅ Flask metric emitter exposing `data_drift_score`
* ✅ Prometheus scraping metrics
* ✅ Prometheus datasource pre-provisioned in Grafana

Your task is to create a **Grafana dashboard** containing a **Table** panel that displays the latest `data_drift_score` for each feature column.

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

## Step 2 - Create a Dashboard

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

Select the pre-provisioned **Prometheus** datasource.

## Step 3 - Configure the Table Panel

Visualization type:

```text
Table
```

PromQL query:

```promql
data_drift_score
```

This query returns all series:

```text
data_drift_score{column="amount"}
data_drift_score{column="hour"}
data_drift_score{column="num_tx_past_day"}
```

## Step 4 - Format as a Table

If needed, apply the built-in transformation:

```text
Transform
    ↓
Labels to fields
```

(or the equivalent "Time series to rows" option, depending on the Grafana version.)

The resulting table should resemble:

| column          | Value |
| --------------- | ----: |
| amount          |  0.11 |
| hour            |  0.09 |
| num_tx_past_day |  0.14 |

## Step 5 - Save Dashboard

Click **Save Dashboard**

Example name:

```text
Fraud Detection Data Drift
```

# Solution Steps

## Step 1

Open Grafana:

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
↓

Add Visualization
```

Datasource:

```text
Prometheus
```

## Step 3

Visualization:

```text
Table
```

Query:

```promql
data_drift_score
```

## Step 4

(Optional)

Apply transformation:

```text
Labels to fields
```

to display one row per feature.

<img width="1512" height="841" alt="dashboard ui" src="https://github.com/user-attachments/assets/53c121b8-3665-40c3-8a22-c17ca68298ac" />

## Step 5

Save dashboard.

# Verification

## Verify Dashboard Exists

```bash
curl -u admin:grafana2026 \
"http://localhost:3000/api/search?type=dash-db"
```

Expected:

At least one dashboard.

<img width="731" height="152" alt="search" src="https://github.com/user-attachments/assets/cd57611c-1291-43ff-af53-8857d66fa306" />

## Verify Dashboard JSON

Obtain dashboard UID:

```bash
curl -u admin:grafana2026 \
"http://localhost:3000/api/search?type=dash-db"
```

Then inspect it:

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/dashboards/uid/<dashboard_uid>
```

Expected:

Panel type:

```text
table
```

Query:

```promql
data_drift_score
```

## Verify Prometheus Returns Data

Use Grafana's datasource proxy (replace `<DATASOURCE_UID>` with your Prometheus datasource UID):

```bash
curl -u admin:grafana2026 \
"http://localhost:3000/api/datasources/proxy/uid/<DATASOURCE_UID>/api/v1/query?query=data_drift_score"
```

Expected response contains series similar to:

```json
{
  "metric": {
    "__name__": "data_drift_score",
    "column": "amount"
  }
}
```

```json
{
  "metric": {
    "__name__": "data_drift_score",
    "column": "hour"
  }
}
```

```json
{
  "metric": {
    "__name__": "data_drift_score",
    "column": "num_tx_past_day"
  }
}
```

This confirms that Grafana receives per-feature rows for the table.

<img width="730" height="231" alt="dashboard cli" src="https://github.com/user-attachments/assets/c7c4964c-3844-410b-b6f9-43f44311593e" />

# Workflow

```text
        Metric Emitter
              │
              ▼
 data_drift_score{column=*}
              │
              ▼
         Prometheus
        (Scrapes every 5s)
              │
              ▼
     Prometheus Datasource
              │
              ▼
      Grafana Table Panel
              │
              ▼
 Feature Name │ Drift Score
──────────────┼────────────
amount        │ 0.11
hour          │ 0.09
num_tx_past_day │ 0.14
```

# Fun Message 😄

*"Accuracy tells you how the model performs—but a data drift table tells you **why** performance might change. Catch shifting features before they become costly surprises! 📊🔍"*
