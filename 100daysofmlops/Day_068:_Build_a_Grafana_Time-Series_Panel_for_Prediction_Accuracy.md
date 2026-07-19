# Instruction

The xFusionCorp Industries ML Platform team monitors the **rolling prediction accuracy** of their fraud-detection model using **Grafana**.

The monitoring infrastructure is already configured:

* ✅ Metric emitter exposing `prediction_accuracy`
* ✅ Prometheus scraping metrics
* ✅ Prometheus datasource pre-provisioned in Grafana

Your task is to create a **Grafana dashboard** with a **Time Series** panel that visualizes the `prediction_accuracy` metric over time.


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


## Step 2 - Verify the Prometheus Datasource

Navigate:

```text
Connections
    ↓
Data Sources
```

You should already see:

```text
Prometheus
```

No further configuration is needed.


## Step 3 - Create a Dashboard

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

Choose the pre-provisioned **Prometheus** datasource.


## Step 4 - Create the Time Series Panel

Panel Type:

```text
Time Series
```

PromQL query:

```promql
prediction_accuracy
```

The graph should display a rolling accuracy value that changes over time.

<img width="1510" height="821" alt="setup" src="https://github.com/user-attachments/assets/a5a4ccfa-a5b7-49e7-80c9-738cb586a369" />

## Step 5 - Save Dashboard

Click:

```text
Save Dashboard
```

Example name:

```text
Fraud Detection Accuracy Monitoring
```


# Solution Steps

## Step 1 - Open Grafana

```text
http://localhost:3000
```

Login using:

```text
admin

grafana2026
```


## Step 2 - Verify Datasource

```text
Connections
↓

Data Sources
```

Expected:

```text
Prometheus
```


## Step 3 - Create Dashboard

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


## Step 4 - Configure Panel

Visualization:

```text
Time Series
```

Query:

```promql
prediction_accuracy
```


## Step 5 - Save Dashboard

Example title:

```text
Fraud Detection Accuracy Monitoring
```

Click **Save**.


# Verification

## Verify Datasource

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/datasources
```

Expected:

```text
Prometheus
```


## Verify Dashboard Exists

```bash
curl -u admin:grafana2026 \
"http://localhost:3000/api/search?type=dash-db"
```

Expected:

At least one dashboard.

<img width="762" height="177" alt="search" src="https://github.com/user-attachments/assets/52aa6724-e130-49a5-88cf-65e9bdb9314c" />

## Verify Dashboard JSON

First obtain the dashboard UID:

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

* At least one panel
* Panel type:

```text
timeseries
```

* Query:

```promql
prediction_accuracy
```

<img width="752" height="272" alt="dashboard cli" src="https://github.com/user-attachments/assets/8f8884bd-391c-4397-ade9-2b7eb7c1099e" />


# Workflow

```text
          Metric Emitter
               │
               ▼
      prediction_accuracy
               │
               ▼
          Prometheus
        (Scrape every 5s)
               │
               ▼
     Prometheus Datasource
               │
               ▼
      Grafana Dashboard
               │
               ▼
     Time Series Visualization
```

# Fun Message 😄

*"A model isn't just judged by today's accuracy—Grafana lets you watch its performance story unfold over time! 📈🤖"*
