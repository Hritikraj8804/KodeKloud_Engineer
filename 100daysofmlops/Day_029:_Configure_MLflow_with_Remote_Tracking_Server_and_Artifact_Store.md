## Instruction

The xFusionCorp Industries ML Platform team deployed a production-style MLflow setup using:

* **PostgreSQL** for tracking metadata
* **SeaweedFS (S3-compatible)** for artifact storage

Although experiment runs were appearing in MLflow successfully, a teammate noticed that model artifacts were never uploaded to the SeaweedFS bucket.

The task was to diagnose the MLflow server configuration, fix the artifact storage integration, restart the server, and verify a complete metadata + artifact round trip.


# Environment Details

| Component          | Details                 |
| ------------------ | ----------------------- |
| PostgreSQL         | `mlflow-db`             |
| Database           | `mlflow`                |
| Username           | `mlflow`                |
| Password           | `mlflow123`             |
| Port               | `5432`                  |
| SeaweedFS S3 API   | `http://localhost:8333` |
| SeaweedFS Filer UI | `http://localhost:8888` |
| Access Key         | `weedadmin`             |
| Secret Key         | `weedadmin123`          |
| Bucket             | `mlflow-artifacts`      |
| MLflow Port        | `5000`                  |


# Solution

## ✅ Add SeaweedFS Endpoint Configuration

Update:

```bash
/root/code/start-mlflow.sh
```

Add:

```bash
export MLFLOW_S3_ENDPOINT_URL=http://localhost:8333
```

<img width="772" height="366" alt="test 1st run" src="https://github.com/user-attachments/assets/4c56199d-df84-48cc-bc7f-40c5a53692c7" />

## 1. Edit Configuration

```bash
vi /root/code/start-mlflow.sh
```

Add:

```bash
#!/bin/bash

set -e

export AWS_ACCESS_KEY_ID=weedadmin
export AWS_SECRET_ACCESS_KEY=weedadmin123
export MLFLOW_S3_ENDPOINT_URL=http://localhost:8333

exec mlflow server \
  --backend-store-uri postgresql://mlflow:mlflow123@localhost:5432/mlflow \
  --artifacts-destination s3://mlflow-artifacts \
  --host 0.0.0.0 --port 5000 \
  --allowed-hosts '*' --cors-allowed-origins '*'
```

<img width="960" height="542" alt="config fix" src="https://github.com/user-attachments/assets/39cec285-b950-40dc-8a4f-d81bfb698047" />

## 2. Restart MLflow

```bash
bash /root/code/restart-mlflow.sh
```

This:

* Stops the existing server
* Reloads environment variables
* Starts MLflow again

<img width="582" height="57" alt="bash run" src="https://github.com/user-attachments/assets/628d2b8f-7581-4c79-8972-b1c1d4607ce0" />

## 3. Verify Server

```bash
ss -tulnp | grep 5000
```

Expected:

```text
0.0.0.0:5000
```

---

## 4. Run Smoke Test

```bash
python3 /root/code/log_test_run.py
```

Expected:

```text
Run completed successfully
```

<img width="767" height="165" alt="run test" src="https://github.com/user-attachments/assets/848e42ac-b047-4065-a4f8-e31bc2e95607" />

# Verification ✅

## Verify MLflow UI

Open:

```text
MLflow UI
```

Expected experiment:

```text
test-remote
```

contains:

```text
FINISHED run
```

<img width="1827" height="817" alt="ui test verify" src="https://github.com/user-attachments/assets/577fa794-47ee-4996-b46a-19a8f672d5ba" />

## Verify SeaweedFS

Open:

```text
SeaweedFS Filer
```

Navigate:

```text
/buckets/mlflow-artifacts/
```

Expected structure:

```text
mlflow-artifacts/
└── artifacts/
    └── model/
        ├── MLmodel
        ├── model.pkl
        ├── conda.yaml
        └── requirements.txt
```

Artifacts now successfully upload.

<img width="1611" height="365" alt="ui bucket verify" src="https://github.com/user-attachments/assets/11d3c4a7-1f25-4da1-9d57-d573d9c082c7" />

## Verify PostgreSQL

Connect:

```bash
docker exec -it mlflow-db psql -U mlflow -d mlflow
```

List tables:

```sql
\dt
```

Expected MLflow schema:

```text
experiments
runs
metrics
params
tags
...
```

Exit:

```sql
\q
```

<img width="702" height="337" alt="psql" src="https://github.com/user-attachments/assets/53941896-88cf-488f-9167-c5538dbb8e28" />

# Fun Message 😄

*"The metadata knew exactly what happened. The artifacts just didn't know where home was 😄📦"*
