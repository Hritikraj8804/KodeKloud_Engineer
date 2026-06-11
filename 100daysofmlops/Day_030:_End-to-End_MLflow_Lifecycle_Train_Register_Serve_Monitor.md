## Instruction

The xFusionCorp Industries MLOps team needed to take the best-performing fraud detection model through the final stages of the ML lifecycle:

1. Identify the best candidate model
2. Register it in the MLflow Model Registry
3. Assign the **champion** alias
4. Serve the model for live inference
5. Verify endpoint health
6. Create a monitoring script

The infrastructure was already available:

* PostgreSQL → MLflow metadata store
* SeaweedFS → MLflow artifact store
* MLflow Tracking Server → Port 5000
* Three candidate runs in the `fraud-detection-v2` experiment



# Architecture

```text
fraud-detection-v2 Experiment
            │
            ▼
     Best F1 Score Run
            │
            ▼
     MLflow Registry
   fraud-detector-v2
            │
      Alias: champion
            │
            ▼
   mlflow models serve
        Port 5001
            │
            ▼
      /health Probe
            │
            ▼
       monitor.sh
```



# Requirements

The final state required:

✅ Registered model:

```text
fraud-detector-v2
```

✅ Champion alias assigned to highest F1-score model

✅ Model serving on:

```text
http://localhost:5001
```

✅ Health endpoint:

```text
GET /health
```

returns:

```http
200 OK
```

✅ Monitoring script:

```bash
/root/code/monitor.sh
```

exists and is executable



# Solution

## Step 1 - Identify the Best Run

### Option A - MLflow UI

Open:

```text
MLflow UI
→ fraud-detection-v2
```

Sort runs by:

```text
metrics.f1_score
```

Identify the run with the highest value.

Example:

| Run        | F1 Score |
| ---------- | -------- |
| baseline   | 0.81     |
| improved   | 0.92 ✅   |
| regression | 0.74     |

Winner:

```text
improved
```



### Option B - Python SDK

```python
from mlflow import MlflowClient

client = MlflowClient("http://localhost:5000")

exp = client.get_experiment_by_name(
    "fraud-detection-v2"
)

runs = client.search_runs(
    [exp.experiment_id],
    order_by=["metrics.f1_score DESC"]
)

print(runs[0].info.run_id)
```



# Step 2 - Register the Model

Open the winning run.

Navigate:

```text
Artifacts
→ model
→ Register Model
```

Create:

```text
fraud-detector-v2
```



# Step 3 - Assign Champion Alias

Open:

```text
Models
→ fraud-detector-v2
```

Select the registered version.

Add alias:

```text
champion
```

Expected:

```text
fraud-detector-v2
└── Version 1
      Alias: champion
```

(or whichever version corresponds to the winning run)

<img width="1337" height="606" alt="campion" src="https://github.com/user-attachments/assets/d349e4f6-c9de-4a6c-b0d1-09e72edd0d39" />

# Step 4 - Export Tracking URI

Before serving:

```bash
export MLFLOW_TRACKING_URI=http://localhost:5000
```

Required because:

```text
models:/fraud-detector-v2@champion
```

must be resolved via the tracking server.

# Step 5 - Serve the Champion Model

Run in background:

```bash
nohup mlflow models serve \
  -m "models:/fraud-detector-v2@champion" \
  -p 5001 \
  --host 0.0.0.0 \
  --env-manager=local \
  > /tmp/mlflow-serving.log 2>&1 &
```

<img width="656" height="175" alt="model serve" src="https://github.com/user-attachments/assets/51b14bcd-15bb-4846-8b4a-f916d5ad73e3" />


# Step 6 - Verify Port 5001

```bash
ss -tulnp | grep 5001
```

Expected:

```text
LISTEN 0 128 0.0.0.0:5001
```



# Step 7 - Verify Health Endpoint

```bash
curl -i http://localhost:5001/health
```

Expected:

```http
HTTP/1.1 200 OK
```



# Step 8 - Create monitor.sh

Create:

```bash
vi /root/code/monitor.sh
```

Contents:

```bash
#!/usr/bin/env bash

set -u

if curl -sf -o /dev/null http://localhost:5001/health; then
    echo "healthy"
    exit 0
fi

echo "unhealthy"
exit 1
```

<img width="652" height="267" alt="monitoring" src="https://github.com/user-attachments/assets/f2bcbca0-7be3-41ec-a792-10a7f6ce1ea4" />


# Step 9 - Make Script Executable

```bash
chmod +x /root/code/monitor.sh
```



# Step 10 - Verify Monitoring Script

Run:

```bash
/root/code/monitor.sh
```

Expected:

```text
healthy
```

Check exit code:

```bash
echo $?
```

Expected:

```text
0
```

<img width="512" height="65" alt="exec" src="https://github.com/user-attachments/assets/64e1d32c-fa3a-472c-8de2-4f734562af59" />


# Verification Checklist ✅

## Registry

```text
Models
└── fraud-detector-v2
```

exists.



## Alias

```text
champion
```

assigned to highest-F1 version.



## Serving

```bash
ss -tulnp | grep 5001
```

shows:

```text
LISTEN
```



## Health Check

```bash
curl http://localhost:5001/health
```

returns:

```http
200
```



## Monitoring Script

```bash
/root/code/monitor.sh
```

returns:

```text
healthy
```

and:

```bash
echo $?
```

returns:

```text
0
```

# Fun Message 😄

*"A model isn't truly promoted until it survives production traffic and a health check at 3 AM 😄🚀"*
