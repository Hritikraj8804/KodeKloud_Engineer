## Instruction

The xFusionCorp Industries ML Platform team uses a **config-driven training pipeline** so model hyperparameters and settings can be changed through YAML files without modifying Python code.

A training project already existed at:

```bash
/root/code/fraud-detection/
```

The trainer itself was working correctly, but the configuration file:

```bash
configs/train_config.yaml
```

contained several issues that prevented a successful run.

The goal was to correct the configuration, execute the training pipeline, log a run to MLflow, and save the trained model inside the project directory.



# Project Structure

```text
fraud-detection/
├── configs/
│   └── train_config.yaml
├── data/
│   └── train.csv
├── models/
├── src/
│   └── models/
│       └── train.py
```



# Given Configuration

```yaml
model:
  type: RandomForest
  n_estimators: 100
  max_depth: 5
  random_state: 42

data:
  train_path: /root/code/fraud-detection/data/train.csv
  target_column: target

output:
  model_path: /root/code/model.pkl

mlflow:
  tracking_uri: http://localhost:5000
  experiment_name: fraud-detection
```

<img width="850" height="371" alt="before config" src="https://github.com/user-attachments/assets/afedce0e-5099-44bc-a8fa-5275f5f9be20" />

# Problems Identified 🚨

# Solution

## Step 1 - Edit Configuration

```bash
vi /root/code/fraud-detection/configs/train_config.yaml
```

<img width="845" height="427" alt="after config" src="https://github.com/user-attachments/assets/1c0a7efa-f15d-4329-a7d9-e9e0138ced9b" />

Apply the corrections above.

## Step 2 - Run Training

```bash
python3 /root/code/fraud-detection/src/models/train.py
```

Expected:

```text
Training completed successfully
```

(or similar output from the trainer)

<img width="837" height="210" alt="run" src="https://github.com/user-attachments/assets/3d0f5ac4-ba7e-4db8-916e-91c495367ae6" />

## Step 3 - Verify Model Creation

Check:

```bash
ls -l /root/code/fraud-detection/models/
```

Expected:

```text
model.pkl
```

## Step 4 - Verify MLflow Run

Open:

```text
MLflow UI
→ fraud-detection
```

Expected:

```text
1 new run
```

## Step 5 - Verify Logged Parameters

The run should contain:

```text
n_estimators = 100
max_depth = 5
random_state = 42
```

under the Parameters section.

<img width="1842" height="847" alt="ui" src="https://github.com/user-attachments/assets/d5566773-dcd3-4137-b9fb-17bcda3aba98" />

# Fun Message 😄

*"Sometimes the model isn't broken—the YAML just convinced it to train on a column that doesn't exist 😄⚙️"*
