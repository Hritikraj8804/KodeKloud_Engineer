# Instruction

The xFusionCorp Industries MLOps team is building a centralized observability layer for the **fraud-detector** service.

The monitoring stack already contains:

* FastAPI fraud-detector service
* Prometheus
* Background synthetic traffic
* Prometheus metrics for requests and latency
* Grafana

Grafana starts with no configuration, so the objective is to:

* Connect Grafana to Prometheus
* Create a `fraud-monitor` dashboard
* Add request-rate and latency panels
* Create an alert rule based on a service metric

# Solution

## Step 1 - Open Grafana

Open the **Grafana UI** on port `3000`.

Login:

```text
Username: admin
Password: admin
```

# Step 2 - Add Prometheus Data Source

Navigate to:

```text
Connections
    ↓
Data sources
    ↓
Add new data source
```

Select:

```text
Prometheus
```

Set the URL to:

```text
http://prometheus:9090
```

Then click:

```text
Save & test
```

Expected:

```text
Successfully queried the Prometheus API.
```

✔ Prometheus is now connected to Grafana.

# Step 3 - Create the Dashboard

Navigate to:

```text
Dashboards
    ↓
New
    ↓
New dashboard
```

Create a dashboard named:

```text
fraud-monitor
```

# Panel 1 - Request Rate 📈

Create the first panel.

Use the Prometheus data source and the following PromQL:

```promql
rate(http_requests_total[1m])
```

This calculates the number of requests per second over the previous minute.

Set the visualization to:

```text
Time series
```

Panel title:

```text
Request Rate
```

Save the panel.

# Panel 2 - Request Latency ⏱️

Create another panel using:

```promql
rate(http_request_duration_seconds_sum[5m])
/
rate(http_request_duration_seconds_count[5m])
```

This calculates the average request duration.

Alternatively, the latency histogram can be queried directly using the available:

```text
http_request_duration_seconds
```

metric family.

Use:

```text
Time series
```

as the visualization.

Panel title:

```text
Request Latency
```

Save the panel.

# Step 4 - Save Dashboard

Click:

```text
Save dashboard
```

Set the dashboard name to:

```text
fraud-monitor
```

The dashboard should now contain at least:

```text
fraud-monitor
├── Request Rate
└── Request Latency
```

<img width="1918" height="548" alt="dashboard" src="https://github.com/user-attachments/assets/b0926858-bd2b-4497-a10a-6dea7a47ed57" />

# Step 5 - Create an Alert Rule 🚨

Navigate to:

```text
Alerting
    ↓
Alert rules
    ↓
New alert rule
```

Create an alert based on a service metric.

For example, an alert based on request traffic can use:

```promql
rate(http_requests_total[1m])
```

Configure the alert so it triggers when the metric satisfies a threshold appropriate for the service.

Example:

```text
Condition:
request rate < 0.1
```

Set a rule name such as:

```text
Fraud Detector Traffic Alert
```

Save the alert rule.

The important requirement is that the alert expression references a service metric, such as:

```text
http_requests_total
```

or:

```text
http_request_duration_seconds
```

<img width="1892" height="832" alt="alert" src="https://github.com/user-attachments/assets/20cbf89b-ac62-421b-ba89-dbdbca733853" />

<img width="642" height="537" alt="check" src="https://github.com/user-attachments/assets/eeacb6e1-35db-45f7-adf2-b580debee4f4" />

# Observability Architecture 📊

```text
                  ┌─────────────────────┐
                  │   Fraud Detector    │
                  │      :8085          │
                  └──────────┬──────────┘
                             │
                         /metrics
                             │
                             ▼
                  ┌─────────────────────┐
                  │     Prometheus      │
                  │       :9090        │
                  └──────────┬──────────┘
                             │
                         PromQL
                             │
                             ▼
                  ┌─────────────────────┐
                  │       Grafana       │
                  │       :3000         │
                  └──────────┬──────────┘
                             │
                ┌────────────┴────────────┐
                ▼                         ▼
        ┌───────────────┐        ┌────────────────┐
        │ fraud-monitor │        │   Alert Rules  │
        │   Dashboard   │        │      🚨        │
        └───────────────┘        └────────────────┘
             │
       ┌─────┴──────┐
       ▼            ▼
 Request Rate    Latency
```

# Fun Message 😄

*"A dashboard tells you the model is having a bad day. An alert makes sure you don't have to stare at the dashboard all day to find out. 📊🚨"*
