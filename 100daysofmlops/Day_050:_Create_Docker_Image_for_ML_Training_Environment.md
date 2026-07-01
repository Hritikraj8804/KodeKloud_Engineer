# Instruction

The xFusionCorp Industries ML Platform team wanted every engineer to train the fraud-detection model in an identical environment.

Instead of installing dependencies manually, the training environment was packaged into a Docker image so anyone could simply run:

```bash
docker run ml-trainer:v1
```

A draft Dockerfile was provided, but the image failed to meet the team's standard.

The task was to:

* Use the correct Python base image
* Install every required Python dependency
* Build the image successfully
* Verify all training imports work inside the container

# Solution Steps

## Step 1 - Open the Project

```bash
cd /root/code/ml-docker
```

## Step 2 - Edit Dockerfile

```bash
vi Dockerfile
```

```dockerfile
FROM python:3.11-slim

WORKDIR /app

RUN pip install --no-cache-dir \
    scikit-learn \
    pandas \
    numpy \
    joblib

COPY train.py /app/train.py

CMD ["python3", "train.py"]
```

Update:

* Base image
* Install joblib

<img width="947" height="345" alt="dockerfile update" src="https://github.com/user-attachments/assets/9adb2b65-7d2b-4e5f-8103-8d722cf63840" />

## Step 3 - Build the Image

```bash
docker build -t ml-trainer:v1 .
```

<img width="747" height="407" alt="docker build" src="https://github.com/user-attachments/assets/9db8f31c-2cda-46a1-80ea-f5fb0fff2306" />

Expected output ends with:

```text
Successfully tagged ml-trainer:v1
```

## Step 4 - Verify Image

```bash
docker images ml-trainer:v1
```

Expected:

```text
REPOSITORY    TAG
ml-trainer    v1
```

## Step 5 - Verify Python Imports

```bash
docker run --rm ml-trainer:v1 \
python3 -c "import sklearn, pandas, numpy, joblib; print('OK')"
```

![Uploading docker run.jpg…]()

Expected output:

```text
OK
```

# Fun Message 😄

*"It works on my machine" ends the moment Docker enters the chat. One image, one environment, endless reproducibility! 🐳🚀*
