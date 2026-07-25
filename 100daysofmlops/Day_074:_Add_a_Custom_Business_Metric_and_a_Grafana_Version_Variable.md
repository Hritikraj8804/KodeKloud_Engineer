# Instruction

The xFusionCorp Industries ML Platform team wants to monitor the **total fraudulent transaction amount (USD)** alongside existing model-serving metrics.

The task has two parts:

1. Extend the Flask metric emitter with a new Prometheus counter:

   * `fraud_amount_usd_total`
   * Labelled by `version`
2. Create a Grafana dashboard that:

   * Uses a template variable named `version`
   * Allows filtering metrics by model version (`v1`, `v2`, or All)

# Solution

## Part 1 - Update `metric_emitter.py`

### 1. Create the Counter

Add the following immediately after `REQUEST_TOTAL`:

```python
FRAUD_AMOUNT_USD_TOTAL = Counter(
    "fraud_amount_usd_total",
    "Total fraudulent transaction amount in USD by model version.",
    labelnames=["version"],
    registry=REGISTRY,
)
```

### 2. Increment the Counter

Inside the existing request loop:

```python
for version in ("v1", "v1", "v1", "v2"):
    REQUEST_TOTAL.labels(
        version=version,
        endpoint="/predict",
        method="POST",
    ).inc()

    INFERENCE_LATENCY.observe(random.uniform(0.005, 0.15))
```

add:

```python
    FRAUD_AMOUNT_USD_TOTAL.labels(version=version).inc(
        random.uniform(50, 500)
    )
```

The complete loop becomes:

```python
for version in ("v1", "v1", "v1", "v2"):
    REQUEST_TOTAL.labels(
        version=version,
        endpoint="/predict",
        method="POST",
    ).inc()

    INFERENCE_LATENCY.observe(random.uniform(0.005, 0.15))

    FRAUD_AMOUNT_USD_TOTAL.labels(
        version=version
    ).inc(random.uniform(50, 500))
```

### 3. Restart the Metric Emitter

```bash
cd /root/code/monitoring

docker compose restart metric-emitter
```

Wait a few seconds for Prometheus to scrape the new metric.

# Part 2 - Configure Grafana Variable

Open Grafana

```text
http://localhost:3000
```

Login

```text
admin
grafana2026
```

Navigate:

```text
Dashboard

↓

Settings

↓

Variables

↓

New Variable
```

Configure:

| Field       | Value                                           |
| ----------- | ----------------------------------------------- |
| Name        | `version`                                       |
| Type        | Query                                           |
| Data source | Prometheus                                      |
| Query       | `label_values(fraud_amount_usd_total, version)` |
| Multi-value | Enabled (optional)                              |
| Include All | Enabled                                         |

<img width="1435" height="862" alt="query variable" src="https://github.com/user-attachments/assets/fc355598-8521-42b3-9f55-4524c56cb1b6" />

Save the variable.

# Part 3 - Create Dashboard Panel

Create a panel.

Example visualization:

```text
Time Series
```

PromQL query:

```promql
rate(fraud_amount_usd_total{version=~"$version"}[1m])
```

Alternative (cumulative total):

```promql
fraud_amount_usd_total{version=~"$version"}
```

<img width="1895" height="497" alt="promethus" src="https://github.com/user-attachments/assets/c425bb20-20bd-430f-af06-3d1fb0addd0f" />

Save the dashboard.

# Solution Steps

## Step 1

Edit the metric emitter:

```bash
cd /root/code/monitoring/app

vi metric_emitter.py
```

## Step 2

Add:

```python
FRAUD_AMOUNT_USD_TOTAL = Counter(
    "fraud_amount_usd_total",
    "Total fraudulent transaction amount in USD by model version.",
    labelnames=["version"],
    registry=REGISTRY,
)
```

## Step 3

Increment inside `_nudge_metrics()`:

```python
FRAUD_AMOUNT_USD_TOTAL.labels(
    version=version
).inc(random.uniform(50, 500))
```

## Step 4

Restart:

```bash
cd /root/code/monitoring

docker compose restart metric-emitter
```

<img width="1027" height="97" alt="docker compose restart" src="https://github.com/user-attachments/assets/ae18b1a0-69d2-4117-bec5-66af2ad2fffb" />

<img width="1012" height="217" alt="docker ps" src="https://github.com/user-attachments/assets/ffd131c0-59b3-47be-bd09-cbe8cd099527" />

## Step 5

Create Grafana variable.

Name:

```text
version
```

Query:

```promql
label_values(fraud_amount_usd_total, version)
```

## Step 6

Create a panel.

Query:

```promql
rate(fraud_amount_usd_total{version=~"$version"}[1m])
```

Save the dashboard.


# Workflow

```text
          Metric Emitter
                │
                ▼
fraud_amount_usd_total{version}
                │
                ▼
           Prometheus
                │
                ▼
 label_values(..., version)
                │
                ▼
      Grafana Variable ($version)
                │
                ▼
fraud_amount_usd_total{version=~"$version"}
                │
                ▼
     Version-Aware Dashboard Panel
```

# Fun Message 😄

*"One metric, many versions—labels and dashboard variables let a single Grafana panel adapt instantly, making multi-model monitoring both scalable and elegant! 📈✨"*
