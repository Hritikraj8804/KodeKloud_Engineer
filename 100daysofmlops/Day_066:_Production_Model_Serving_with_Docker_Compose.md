# Instruction

The xFusionCorp Industries ML Platform team deployed a production-ready fraud-detection serving stack consisting of:

* Flask API
* Prometheus metrics
* Redis rate limiting
* Nginx reverse proxy
* Prometheus monitoring
* Grafana dashboards
* Traffic generator

The stack was pre-staged, but several components were incorrectly wired.

The objective was to:

* Fix Prometheus scraping
* Fix Nginx reverse proxy
* Bring all containers online
* Verify metrics collection
* Create a Grafana dashboard showing the API request rate


# Solution

## Update `prometheus.yml`

Replace:

```yaml
static_configs:
  - targets:
      - model-api:8000
```

with

```yaml
static_configs:
  - targets:
      - model-api:5000
```

## Update `nginx.conf`

Replace:

```nginx
upstream model_backend {
    server model-api:8000;
}
```

with

```nginx
upstream model_backend {
    server model-api:5000;
}
```

## Grafana Dashboard

Create a new dashboard.

Add a panel using the preconfigured **Prometheus** datasource.

Example query:

```promql
rate(flask_http_request_total[1m])
```

or

```promql
sum(rate(flask_http_request_total[1m]))
```

depending on the exported metric names.

Save the dashboard.

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/serving/production
```

## Step 2 - Edit Prometheus Configuration

```bash
vi prometheus.yml
```

Update:

```yaml
targets:
  - model-api:5000
```

## Step 3 - Edit Nginx Configuration

```bash
vi nginx.conf
```

Update:

```nginx
server model-api:5000;
```

## Step 4 - Start the Stack

```bash
docker compose up -d
```

<img width="695" height="160" alt="docker compose" src="https://github.com/user-attachments/assets/e4f5ec9a-9d27-4008-87ca-608e61ae6413" />

## Step 5 - Verify Containers

```bash
docker compose ps
```

Expected:

```text
NAME               STATUS
model-api          Up
prod-redis         Up
prod-nginx         Up
prod-prometheus    Up
prod-grafana       Up
prod-traffic       Up
```

<img width="1307" height="147" alt="docker ps" src="https://github.com/user-attachments/assets/9eea6d24-a39f-4049-95e4-fca5a5073fe5" />

## Step 6 - Verify Metrics Endpoint

```bash
curl http://localhost:5000/metrics
```

Expected:

```text
# HELP ...
# TYPE ...
flask_http_request_total ...
```

<img width="697" height="287" alt="curl" src="https://github.com/user-attachments/assets/38494eba-f1fe-4c91-9ea2-d901c3d6f2ae" />

## Step 7 - Verify Prediction API

```bash
curl -X POST http://localhost:8085/predict \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
```

Expected:

```json
{
  "is_fraud": 1
}
```

<img width="652" height="80" alt="perdict" src="https://github.com/user-attachments/assets/37c544eb-ece3-4fbd-9adf-063f7833b7ee" />

## Step 8 - Verify Prometheus Target

```bash
curl http://localhost:9090/api/v1/targets
```

Expected:

```text
health":"up"
```

<img width="1317" height="116" alt="target" src="https://github.com/user-attachments/assets/007bb598-8ec6-4a3e-ab03-ddde60dee6d1" />

for the **model-api** job.

## Step 9 - Verify Grafana Datasource

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/datasources
```

Expected:

```text
Prometheus
```

<img width="737" height="66" alt="grafana" src="https://github.com/user-attachments/assets/f36c7fe3-3688-44f6-aad0-23284c2b66e5" />

## Step 10 - Create Dashboard

Open Grafana.

Navigate:

```
Dashboards
    →
New Dashboard
    →
Add Visualization
```

Choose the preconfigured **Prometheus** datasource.

Use query:

```promql
rate(flask_http_request_total[1m])
```

<img width="1910" height="861" alt="grafana ui" src="https://github.com/user-attachments/assets/94dc1480-da14-46f8-a669-080cb9458216" />

Save the dashboard.

## Step 11 - Verify Dashboard

```bash
curl -u admin:grafana2026 \
"http://localhost:3000/api/search?type=dash-db"
```

<img width="737" height="66" alt="grafana" src="https://github.com/user-attachments/assets/c92bff1d-4ac1-40ef-8c0a-a118c07e7be8" />

Expected:

At least one dashboard.

# Workflow

```text
              Client
                 │
                 ▼
         Nginx (8085)
                 │
                 ▼
        Flask Model API
      /health /predict /metrics
         │           │
         │           ▼
         │        Redis
         │
         ▼
 Prometheus Scraper
         │
         ▼
     Prometheus
         │
         ▼
      Grafana
         │
         ▼
 Request Rate Dashboard
```

# Fun Message 😄

*"Deploying a model is only half the journey—observability lets you see every prediction, every request, and every anomaly before your users do! 📊🚀"*
