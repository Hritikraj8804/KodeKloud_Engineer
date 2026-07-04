# Instruction

The xFusionCorp Industries ML Platform team packages their PyTorch training environment as a Docker image named:

```text
dl-trainer:v1
```

Since every lab host is **CPU-only**, the Docker image must:

* Install the **CPU-only PyTorch wheels**
* Run successfully without requiring CUDA
* Print the installed PyTorch version and whether CUDA is available

The provided Dockerfile targeted the GPU wheel index and expected CUDA to exist, causing both the image build and the container startup to fail.

# Solution Steps

Replace the Dockerfile with:

```dockerfile
FROM python:3.11-slim

WORKDIR /app

RUN pip install --no-cache-dir \
    --index-url https://download.pytorch.org/whl/cpu \
    torch

CMD ["python3", "-c", "import torch; print(torch.__version__, 'cuda?', torch.cuda.is_available())"]
```

## Step 1 - Open Project

```bash
cd /root/code/dl-docker
```

## Step 2 - Edit Dockerfile

```bash
vi Dockerfile
```

Update:

* CPU wheel index
* Remove CUDA assertion
* Print CUDA availability instead

## Step 3 - Build Image

```bash
docker build -t dl-trainer:v1 .
```

Expected output:

```text
Successfully tagged dl-trainer:v1
```

<img width="732" height="342" alt="docker build" src="https://github.com/user-attachments/assets/5dbd44d4-02b1-461d-aa29-f902ac803679" />

## Step 4 - Verify Image

```bash
docker images dl-trainer:v1
```

Expected:

```text
REPOSITORY     TAG

dl-trainer     v1
```

<img width="737" height="112" alt="docker image" src="https://github.com/user-attachments/assets/b75e7e37-f3f5-438f-97db-7814f33908dc" />

## Step 5 - Run Container

```bash
docker run --rm dl-trainer:v1
```

Expected output (version may vary):

```text
2.x.x cuda? False
```

The important part is:

```text
cuda? False
```

and the container exits successfully with status `0`.

<img width="747" height="126" alt="docker run" src="https://github.com/user-attachments/assets/40bc2546-6623-4efc-932b-fbc429f4ff8e" />

# Fun Message 😄

*"Not every model needs a GPU to shine. A well-built CPU image gets the job done anywhere—from your laptop to the lab—with zero CUDA drama! 🐳🔥"*
