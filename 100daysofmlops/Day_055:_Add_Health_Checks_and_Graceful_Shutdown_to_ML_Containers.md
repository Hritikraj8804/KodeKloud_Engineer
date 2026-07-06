# Instruction

The xFusionCorp Industries ML Platform team deploys Flask-based inference APIs as Docker containers. To allow operators to quickly determine whether a container is healthy, every image must include a proper **Docker HEALTHCHECK** and expose the application's listening port.

The provided Dockerfile was almost complete, but it contained two issues:

* The health check targeted a non-existent endpoint (`/healthz`).
* The image did not declare the service port using `EXPOSE`.

# Problem Analysis 🔍

The original Dockerfile contained:

```dockerfile
HEALTHCHECK --interval=5s --timeout=3s --start-period=3s --retries=3 \
  CMD python3 -c "import urllib.request; urllib.request.urlopen('http://localhost:8085/healthz')" || exit 1
```

However, the Flask application exposes:

```text
GET /health
```

There is **no `/healthz` endpoint**, causing every health probe to fail and the container to remain **unhealthy**.

The Dockerfile also lacked:

```dockerfile
EXPOSE 8085
```

Without it, Docker image metadata does not advertise the intended service port.

# Solution

Update the Dockerfile to:

* Add `EXPOSE 8085`
* Point the health probe to `/health`

## Final Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

RUN pip install --no-cache-dir flask

COPY app.py /app/app.py

EXPOSE 8085

HEALTHCHECK --interval=5s --timeout=3s --start-period=3s --retries=3 \
  CMD python3 -c "import urllib.request; urllib.request.urlopen('http://localhost:8085/health')" || exit 1

CMD ["python3", "/app/app.py"]
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/ml-health
```

## Step 2 - Edit Dockerfile

```bash
vi Dockerfile
```

Make the following changes:

* Add

```dockerfile
EXPOSE 8085
```

* Change

```text
/healthz
```

to

```text
/health
```

## Step 3 - Build Image

```bash
docker build -t ml-health:v1 .
```

<img width="747" height="286" alt="docker build" src="https://github.com/user-attachments/assets/d154cde1-ec6e-4c51-90b6-f0d59b34d6a0" />

## Step 4 - Run Container

```bash
docker run -d --rm \
  --name ml-health-api \
  -p 8085:8085 \
  ml-health:v1
```

# Verification

## Verify Container is Running

```bash
docker ps
```

Expected:

```text
ml-health-api
```

<img width="732" height="90" alt="docker run" src="https://github.com/user-attachments/assets/dc7b9595-41f3-4d82-a131-1c641ce0c00b" />

## Verify Health Endpoint

```bash
curl http://localhost:8085/health
```

Expected:

```json
{
  "status": "ok"
}
```

## Verify HTTP Status

```bash
curl -i http://localhost:8085/health
```

Expected:

```text
HTTP/1.1 200 OK
```

<img width="722" height="95" alt="verify" src="https://github.com/user-attachments/assets/e7629698-6ca5-45e0-bc3d-16055e5843bc" />

## Verify Docker Health

Wait around 10–15 seconds, then run:

```bash
docker inspect --format='{{.State.Health.Status}}' ml-health-api
```

Expected:

```text
healthy
```

## Verify Exposed Port

```bash
docker inspect --format '{{.Config.ExposedPorts}}' ml-health:v1
```

Expected:

```text
map[8085/tcp:{}]
```

<img width="731" height="77" alt="docker inspect" src="https://github.com/user-attachments/assets/934520c5-e253-4103-9b15-55a802263037" />

# Fun Message 😄

*"A running container isn't always a healthy container. With a proper HEALTHCHECK, Docker becomes your first line of defense against silent failures! 🩺🐳"*
