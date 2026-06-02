## Instruction

The xFusionCorp Industries data science team needed a baseline experiment recorded in MLflow.

A script already existed at:

```bash
/root/code/log_experiment.py
```

The script:

* Created a parameter dictionary
* Trained a simple Scikit-Learn model
* Generated evaluation metrics

However, the MLflow integration was incomplete. Three `# TODO` sections inside the `mlflow.start_run()` block needed to be implemented.

The goal was to ensure that:

* All parameters were logged
* All metrics were logged
* The trained model was stored as an MLflow artifact

After completing the script, the run needed to be executed once and verified in the MLflow UI.


# Requirements

The resulting MLflow run had to contain:

### Parameters

```python
n_estimators = 100
max_depth = 5
random_state = 42
```

### Metrics

```python
accuracy
f1_score
```

### Artifacts

```python
trained sklearn model
```

The run should appear under the:

```text
Default
```

experiment in the MLflow dashboard.


# Solution

## ✅ Step 1 - Open the Script

```bash
vi /root/code/log_experiment.py
```

Locate the section:

```python
with mlflow.start_run():

    # TODO

    # TODO

    # TODO
```


# Step 2 - Log Parameters

Add:

```python
mlflow.log_params(params)
```

This records:

```text
n_estimators=100
max_depth=5
random_state=42
```

inside the run.


# Step 3 - Log Metrics

Add:

```python
mlflow.log_metric("accuracy", accuracy)
mlflow.log_metric("f1_score", f1_score)
```

This stores both evaluation scores.


# Step 4 - Log the Model Artifact

Add:

```python
mlflow.sklearn.log_model(model, "model")
```

This uploads the trained Scikit-Learn model as an MLflow artifact.


# Example Completed Block

```python
with mlflow.start_run():

    mlflow.log_params(params)

    mlflow.log_metric("accuracy", accuracy)
    mlflow.log_metric("f1_score", f1_score)

    mlflow.sklearn.log_model(model, "model")
```

<img width="627" height="410" alt="config" src="https://github.com/user-attachments/assets/0d09330e-daaf-420d-8afd-0d19c82aff33" />


# Step 5 - Execute the Script

Run:

```bash
python3 /root/code/log_experiment.py
```

Expected output:

```text
Experiment logged successfully
```

(or similar depending on the script)


<img width="755" height="218" alt="run" src="https://github.com/user-attachments/assets/67d1036f-cc38-4194-b4fd-431250e54385" />


# Step 6 - Verify the Run in MLflow

Open the **MLflow UI** button.

Navigate to:

```text
Default Experiment
```

Expected:

```text
Runs: 1
```


# Verify Parameters

Under the run's **Parameters** section:

```text
n_estimators = 100
max_depth = 5
random_state = 42
```


# Verify Metrics

Under **Metrics**:

```text
accuracy
f1_score
```

with their recorded values.


# Verify Artifacts

Under **Artifacts**:

```text
model/
```

The MLflow model package should be present.


<img width="1815" height="892" alt="mlflow" src="https://github.com/user-attachments/assets/af194d5d-56d7-44b0-accb-ced5116a2f76" />


# Fun Message 😄

*"If it's not logged in MLflow, did the experiment even happen? 😄📊"*
