# Instruction

The xFusionCorp Industries ML Platform team wanted a reproducible local MLOps development environment where every engineer could launch the complete stack with a single command.

The development stack consisted of:

* 📓 JupyterLab
* 📊 MLflow
* 🗂️ SeaweedFS (S3-compatible object storage)

A draft `docker-compose.yml` was provided, but the services were not exposed on the team's standard ports.

The task was to correct the Docker Compose configuration, start the stack, and verify that every web interface was accessible.

# Solution

## Updated Jupyter Service

Add the startup command:

```yaml
command: >
  start-notebook.py
  --IdentityProvider.token=''
  --ServerApp.password=''
```

This disables token authentication.

## Updated SeaweedFS Ports

Replace:

```yaml
ports:
  - "9001:8333"
  - "9000:8888"
```

with:

```yaml
ports:
  - "9000:8333"
  - "9001:8888"
```

# Final docker-compose.yml

```yaml
services:
  jupyter:
    image: jupyter/base-notebook:python-3.11
    container_name: ml-jupyter
    ports:
      - "8888:8888"
    volumes:
      - ./notebooks:/home/jovyan/work
    command: >
      start-notebook.py
      --IdentityProvider.token=''
      --ServerApp.password=''

  mlflow:
    image: ghcr.io/mlflow/mlflow:v2.15.1
    container_name: ml-mlflow
    ports:
      - "5000:5000"
    volumes:
      - mlflow-data:/mlflow
    command: >-
      mlflow server
      --host 0.0.0.0
      --port 5000
      --backend-store-uri sqlite:////mlflow/mlflow.db
      --default-artifact-root /mlflow/artifacts

  seaweedfs:
    image: chrislusf/seaweedfs:4.22
    container_name: ml-seaweedfs
    ports:
      - "9000:8333"
      - "9001:8888"
    volumes:
      - seaweedfs-data:/data
    command: server -dir=/data -s3

volumes:
  mlflow-data:
  seaweedfs-data:
```

# Solution Steps

## Step 1 - Open Project

```bash
cd /root/code/ml-dev
```

## Step 2 - Edit Compose File

```bash
vi docker-compose.yml
```

Update:

* SeaweedFS port mappings
* Jupyter startup command

## Step 3 - Start the Stack

```bash
docker compose up -d
```

Expected:

```text
Creating ml-jupyter
Creating ml-mlflow
Creating ml-seaweedfs

Done
```

## Step 4 - Verify Containers

```bash
docker compose ps
```

Expected:

```text
ml-jupyter     Up

ml-mlflow      Up

ml-seaweedfs   Up
```

<img width="830" height="327" alt="docker compose up" src="https://github.com/user-attachments/assets/63d94ea1-22cc-4c07-8e9e-413cdd0cbc97" />

## Step 5 - Verify Jupyter

```bash
curl http://localhost:8888/
```

Expected:

```text
HTTP/1.1 200 OK
```

or

```text
302 Found
```

without a login prompt.

<img width="1075" height="510" alt="jyupyter" src="https://github.com/user-attachments/assets/68d4cf50-b3da-4d12-8723-62235a3fb849" />

## Step 6 - Verify MLflow

```bash
curl http://localhost:5000/
```

Expected:

```text
HTTP/1.1 200 OK
```

<img width="1007" height="545" alt="mlflow verify" src="https://github.com/user-attachments/assets/b4126d7c-5ee7-47cd-b6d5-9218d33e4abb" />

## Step 7 - Verify SeaweedFS

```bash
curl http://localhost:9001/
```

Expected:

```text
200
```

or

```text
302
```

or

```text
403
```

Any of these indicate the Filer UI is responding.

<img width="1237" height="472" alt="seaweed verify" src="https://github.com/user-attachments/assets/d95c9ab4-f905-4432-8514-deb654da3626" />

<img width="761" height="580" alt="curl verify" src="https://github.com/user-attachments/assets/f28b99f7-5228-466c-bbad-8f5c3ac8423c" />

# Fun Message 😄

*"One command. Three services. Zero setup headaches. That's the magic of Docker Compose—your entire MLOps lab comes alive before your coffee gets cold! 🐳☕"*
