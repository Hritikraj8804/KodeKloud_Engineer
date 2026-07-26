# Instruction

The xFusionCorp Industries ML Platform team's monitoring stack is broken.

Although the **Evidently drift scorer** is working correctly, none of its metrics reach Grafana because of **three integration issues**.

Your objectives are:

1. Fix the Flask metric emitter so `/metrics` returns metrics.
2. Fix Prometheus so the metric-emitter target becomes **UP**.
3. Fix Grafana so it connects to Prometheus correctly.
4. Create a tagged monitoring dashboard with at least three panels.

# Solution

## Step 1 - Fix the Metric Emitter

Test the endpoint:

```bash
curl -i http://localhost:5000/metrics
```

Current result:

```text
HTTP/1.1 404 NOT FOUND
```

<img width="982" height="237" alt="curl before" src="https://github.com/user-attachments/assets/4ff041ed-c60a-48ef-8579-cffbc1d5d5b9" />

A healthy exporter must expose:

```python
@app.route("/metrics")
def metrics():
    return generate_latest(REGISTRY), 200, {
        "Content-Type": CONTENT_TYPE_LATEST
    }
```

<img width="982" height="427" alt="metric emitter fix" src="https://github.com/user-attachments/assets/f9795b45-1308-4558-b074-39f358da029b" />

If `/metrics` returns 404, inspect **`app/metric_emitter.py`** and verify:

* Route path is exactly:

```python
@app.route("/metrics")
```

* The function returns:

```python
generate_latest(REGISTRY)
```

After fixing:

```bash
docker compose restart metric-emitter
```

Verify:

```bash
curl -sf http://localhost:5000/metrics
```

Expected:

```
HTTP 200
```

<img width="857" height="227" alt="curl after" src="https://github.com/user-attachments/assets/99f5aaeb-0421-4466-9624-94fdebfa6721" />

## Step 2 - Fix Prometheus

If Prometheus reports:

```text
metric-emitter

DOWN
```

inspect:

```text
prometheus.yml
```

The scrape target should use the Docker Compose service name:

```yaml
scrape_configs:
  - job_name: metric-emitter

    static_configs:
      - targets:
          - metric-emitter:5000
```

Do **not** use:

```text
localhost:5000
```

<img width="837" height="411" alt="promethus fix" src="https://github.com/user-attachments/assets/2478c53d-f065-4826-98b4-bf682d36c7bd" />

because inside the Prometheus container, `localhost` refers to Prometheus itself.

Reload Prometheus:

```bash
docker compose restart prometheus
```

Verify:

```bash
curl http://localhost:9090/api/v1/targets
```

Expected:

```json
"health":"up"
```

## Step 3 - Fix Grafana

If panels remain empty even after Prometheus has data, inspect the Prometheus datasource.

Navigate:

```text
Connections
↓

Data Sources
↓

Prometheus
```

Datasource URL must be:

```text
http://prometheus:9090
```

**NOT**

```text
http://localhost:9090
```

Save and test.

Expected:

```text
Datasource is working
```

API verification:

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/datasources
```

Expected URL:

```text
http://prometheus:9090
```

<img width="1056" height="102" alt="promethus target" src="https://github.com/user-attachments/assets/dbbac3c8-edcb-4247-8535-37aede492b49" />

# Build Monitoring Dashboard

Create a new dashboard.

Title:

```text
Monitoring Overview
```

Tags:

```text
mlops
monitoring
```

## Panel 1 - Request Rate

Visualization:

```text
Time Series
```

Query:

```promql
sum(rate(flask_http_request_total[1m]))
```

## Panel 2 - p95 Latency

Visualization:

```text
Stat
```

Query:

```promql
histogram_quantile(
  0.95,
  sum(rate(model_inference_duration_seconds_bucket[5m])) by (le)
)
```

## Panel 3 - Prediction Accuracy

Visualization:

```text
Gauge
```

Query:

```promql
prediction_accuracy
```

(Optional)

Replace the third panel with:

```promql
data_drift_score
```

if drift monitoring is preferred.

<img width="1907" height="896" alt="grafana ui" src="https://github.com/user-attachments/assets/a2dfe120-dc7c-442f-9626-4cddb851880c" />

<img width="1045" height="667" alt="grafana tags" src="https://github.com/user-attachments/assets/ed2e521b-1a36-4c22-ac26-1acf38ee833e" />

Save the dashboard.


# Workflow

```text
     Evidently Drift Scorer
               │
               ▼
      Flask Metric Emitter
      (/metrics → HTTP 200)
               │
               ▼
        Prometheus Target
          (health = up)
               │
               ▼
   Grafana Prometheus Datasource
(http://prometheus:9090)
               │
               ▼
     Tagged Monitoring Dashboard
```

# Fun Message 😄

*"Monitoring is a chain of trust—if one link breaks, every dashboard goes silent. Fix the wiring, and the whole observability pipeline comes back to life! 🔗📈"*
