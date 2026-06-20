# Instruction

The xFusionCorp Industries ML Platform team trains fraud-detection models using a lightweight **PyTorch neural network**.

The challenge was to make the training code portable across environments:

* Production clusters may have NVIDIA GPUs available.
* Development and lab environments may be CPU-only.

The existing trainer assumed CUDA was always present and immediately crashed on CPU-only systems.

The task was to:

* Detect the available device automatically.
* Train successfully on CPU or GPU.
* Log the actual device used to MLflow.
* Save the trained PyTorch model.

# Project Structure

```text
fraud-detection/
├── data/
│   └── train.csv
├── models/
│   └── fraud_model.pt
└── src/
    └── models/
        └── train_pytorch.py
```

# Problem Analysis 🔍

## Issue #1 - Hardcoded CUDA Usage

The scaffold contained:

```python
model = FraudNet()
model = model.cuda()
```

and later:

```python
xb = X_t.cuda()
yb = y_t.cuda()
```

This works only when:

```python
torch.cuda.is_available() == True
```

In this lab:

```python
torch.cuda.is_available() == False
```

Result:

```text
RuntimeError: CUDA unavailable
```

# Issue #2 - Incorrect MLflow Logging

The scaffold always logged:

```python
mlflow.log_param("device", "cuda")
```

Even when training occurred on CPU.

This made experiment tracking inaccurate.

# Solution

Use PyTorch's standard device detection pattern:

```python
device = torch.device(
    "cuda" if torch.cuda.is_available() else "cpu"
)
```

Then move:

* Model
* Features
* Labels

using:

```python
.to(device)
```

Finally log:

```python
str(device)
```

to MLflow.

# Fix #1 - Create Device Object

Add near the start of `main()`:

```python
device = torch.device(
    "cuda" if torch.cuda.is_available() else "cpu"
)
```

# Fix #2 - Move Model to Device

Replace:

```python
model = FraudNet()
model = model.cuda()
```

with:

```python
model = FraudNet().to(device)
```

# Fix #3 - Move Tensors to Device

Replace:

```python
xb = X_t.cuda()
yb = y_t.cuda()
```

with:

```python
xb = X_t.to(device)
yb = y_t.to(device)
```

# Fix #4 - Log Actual Device

Replace:

```python
mlflow.log_param("device", "cuda")
```

with:

```python
mlflow.log_param(
    "device",
    str(device)
)
```

# Corrected Sections

## Device Selection

```python
device = torch.device(
    "cuda" if torch.cuda.is_available()
    else "cpu"
)
```

## Model Placement

```python
model = FraudNet().to(device)
```

## Tensor Placement

```python
xb = X_t.to(device)
yb = y_t.to(device)
```

## MLflow Logging

```python
mlflow.log_param(
    "device",
    str(device)
)
```

# Final Device-Aware Pattern

```python
device = torch.device(
    "cuda" if torch.cuda.is_available()
    else "cpu"
)

model = FraudNet().to(device)

xb = X_t.to(device)
yb = y_t.to(device)

mlflow.log_param(
    "device",
    str(device)
)
```

# Solution Steps

## Step 1 - Edit Trainer

```bash
vi /root/code/fraud-detection/src/models/train_pytorch.py
```

Apply all fixes.

## Step 2 - Run Training

```bash
python3 /root/code/fraud-detection/src/models/train_pytorch.py
```

Expected output:

```text
epoch 00  loss=...
epoch 01  loss=...
...
epoch 29  loss=...

Model saved to
/root/code/fraud-detection/models/fraud_model.pt
```

<img width="737" height="266" alt="run" src="https://github.com/user-attachments/assets/3a3c170d-ea94-434c-9e10-9aad34a66eb9" />

# Verification

## Verify Model File

```bash
ls -l /root/code/fraud-detection/models/fraud_model.pt
```

Expected:

```text
fraud_model.pt
```

exists.

## Verify MLflow Run

Open:

```text
MLflow UI
→ gpu-training
```

Expected:

```text
1 Run
```

### Logged Parameters

```text
device = cpu
```

Since:

```python
torch.cuda.is_available()
```

returns:

```text
False
```

in this lab.

<img width="1672" height="397" alt="ui" src="https://github.com/user-attachments/assets/7be9275b-a2db-4e7d-85be-a5c24c0467da" />

### Logged Metrics

```text
final_loss
```

must be present.

## Verify No CUDA Calls Remain

Search:

```bash
grep -n "cuda()" \
/root/code/fraud-detection/src/models/train_pytorch.py
```

Expected:

```text
(no output)
```

No hardcoded CUDA calls remain.

# Fun Message 😄

*"Good ML code doesn't ask for a GPU. It politely checks whether one is available first 😄🚀"*
