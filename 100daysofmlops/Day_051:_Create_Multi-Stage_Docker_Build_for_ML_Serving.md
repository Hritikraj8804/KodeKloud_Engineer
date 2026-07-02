# Instruction

The xFusionCorp Industries ML Platform team wanted to optimize the fraud-detection serving image.

The existing Docker image worked correctly, but it contained:

* Training source code
* Training dependencies
* Serving dependencies

A production serving image should only include what is required to **serve predictions**, not to train the model.

The task was to refactor the existing **single-stage Dockerfile** into a **multi-stage Docker build**.

# Solution

Convert the Dockerfile into two stages.

## Final Dockerfile

```dockerfile
# ---------- Builder Stage ----------
FROM python:3.11-slim AS builder

WORKDIR /app

RUN pip install --no-cache-dir \
    scikit-learn \
    pandas \
    numpy \
    joblib

COPY train_model.py /app/train_model.py

RUN python3 /app/train_model.py

# ---------- Runtime Stage ----------
FROM python:3.11-slim

WORKDIR /app

RUN pip install --no-cache-dir \
    flask \
    joblib \
    numpy \
    scikit-learn

COPY --from=builder /app/model.pkl /app/model.pkl
COPY serve.py /app/serve.py

EXPOSE 8080

CMD ["python3", "/app/serve.py"]
```

# Solution Steps

## Step 1 - Open Project

```bash
cd /root/code/ml-serve
```

## Step 2 - Edit Dockerfile

```bash
vi Dockerfile
```

Replace the single-stage build with the multi-stage version.

## Step 3 - Build Image

```bash
docker build -t ml-serve:v1 .
```

<img width="761" height="335" alt="docker build" src="https://github.com/user-attachments/assets/6278001c-932e-44d3-b854-4862df86830a" />

Expected output:

```text
Successfully tagged ml-serve:v1
```

## Step 4 - Verify Image

```bash
docker images ml-serve:v1
```

<img width="780" height="85" alt="docker img" src="https://github.com/user-attachments/assets/93a60c78-0bb2-45bd-ae07-6ce8beb4d1a0" />

Expected:

```text
REPOSITORY     TAG

ml-serve       v1
```

## Step 5 - Run Container

```bash
docker run --rm -p 8088:8080 ml-serve:v1
```

<img width="737" height="162" alt="docker run" src="https://github.com/user-attachments/assets/6dc0a496-1de4-4583-a9e1-9060a355b3e3" />

## Step 6 - Verify Health Endpoint

In another terminal:

```bash
curl http://localhost:8088/health
```

Expected:

```json
{
  "status": "ok"
}
```

<img width="696" height="121" alt="verify" src="https://github.com/user-attachments/assets/9b48e017-e5d0-4298-aef6-6abe6a416f19" />

# Fun Message 😄

*"Train big, ship small! Multi-stage Docker builds let your model graduate from the training lab into production without carrying all the extra baggage. 🐳🤖"*
