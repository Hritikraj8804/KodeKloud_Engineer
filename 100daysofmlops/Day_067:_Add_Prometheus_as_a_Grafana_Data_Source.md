# Instruction

The xFusionCorp Industries ML Platform team already has the following monitoring stack running:

* **Metric Emitter (Flask)** exposing Prometheus metrics
* **Prometheus** scraping metrics every 5 seconds
* **Grafana** running with **no data source configured**

Your objective is to:

* Configure **Prometheus** as a Grafana data source
* Verify the connection
* Create a dashboard
* Add at least one panel that queries a live Prometheus metric
* Save the dashboard


# Solution

## Step 1 - Open Grafana

Open

```
http://localhost:3000
```

Login

```
Username:
admin

Password:
grafana2026
```

## Step 2 - Add Prometheus Data Source

Navigate:

```
Connections
    →
Data Sources
    →
Add data source
```

Choose

```
Prometheus
```

Fill in:

| Field | Value                    |
| ----- | ------------------------ |
| URL   | `http://prometheus:9090` |

<img width="1397" height="756" alt="datasource form" src="https://github.com/user-attachments/assets/e27f5793-0b51-466e-972d-c536442c843f" />

Leave the remaining settings at their defaults.

Click

```
Save & Test
```

Expected message:

```
Successfully queried the Prometheus API

Data source is working
```

<img width="1077" height="326" alt="save and test" src="https://github.com/user-attachments/assets/422c8743-5991-4925-b147-aba2d977f06f" />

## Step 3 - Create Dashboard

Navigate

```
Dashboards
      →
New
      →
New Dashboard
```

Click

```
Add visualization
```

Choose the **Prometheus** datasource.

## Step 4 - Enter a PromQL Query

A simple live metric is:

```promql
rate(flask_http_request_total[1m])
```

Alternative valid queries include:

```promql
prediction_accuracy
```

```promql
data_drift_score
```

```promql
histogram_quantile(
0.95,
rate(model_inference_duration_seconds_bucket[5m])
)
```

<img width="1116" height="552" alt="dashboard" src="https://github.com/user-attachments/assets/04381b5c-e328-49f7-a85e-d79658ad043f" />

Since the metric emitter updates every **5 seconds**, the graph should immediately display changing values.

## Step 5 - Save Dashboard

Click

```
Save dashboard
```

Example name:

```
Fraud Detection Monitoring
```

# Solution Steps

## Step 1 - Verify Stack

```bash
cd /root/code/monitoring

docker compose ps
```

Expected:

```
metric-emitter

mon-prometheus

mon-grafana
```

Running.

## Step 2 - Open Grafana

```
http://localhost:3000
```

Login:

```
admin

grafana2026
```

## Step 3 - Configure Data Source

```
Connections

↓

Data Sources

↓

Add Data Source

↓

Prometheus
```

URL:

```
http://prometheus:9090
```

Click

```
Save & Test
```

## Step 4 - Create Dashboard

```
Dashboards

↓

New Dashboard

↓

Add Visualization
```

Datasource:

```
Prometheus
```

Query:

```promql
rate(flask_http_request_total[1m])
```

Save dashboard.

# Verification

## Verify Data Source

Grafana should report:

```
Data source is working
```

## Verify Health API

Obtain the datasource UID:

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/datasources
```

Example response:

```json
[
  {
    "uid":"P123ABCDEF"
  }
]
```

Then verify its health:

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/datasources/uid/P123ABCDEF/health
```

Expected:

```json
{
  "status":"OK"
}
```

<img width="730" height="127" alt="health" src="https://github.com/user-attachments/assets/cb39d107-33cf-48f3-b5f0-24489b1773f1" />

## Verify Dashboard

```bash
curl -u admin:grafana2026 \
"http://localhost:3000/api/search?type=dash-db"
```

Expected:

At least one dashboard.

<img width="722" height="137" alt="search" src="https://github.com/user-attachments/assets/06ba5049-a39d-4e20-8938-5623b257d919" />

## Verify Dashboard JSON

Find the dashboard UID:

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/search?type=dash-db
```

Then:

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/dashboards/uid/<dashboard_uid>
```

Expected:

* Dashboard contains at least one panel.
* Panel contains a non-empty PromQL query.

<img width="731" height="171" alt="datasource" src="https://github.com/user-attachments/assets/7cf91648-40f4-47b8-b0bd-620cea48959e" />

# Workflow

```text
           Metric Emitter
          (/metrics endpoint)
                  │
                  ▼
            Prometheus
        (scrapes every 5 sec)
                  │
                  ▼
             Grafana
        (Prometheus datasource)
                  │
                  ▼
      Dashboard Visualization
                  │
                  ▼
      Live ML Monitoring Panels
```

# Fun Message 😄

*"Metrics hidden in Prometheus are just numbers—Grafana turns them into stories your operations team can understand at a glance! 📈✨"*
