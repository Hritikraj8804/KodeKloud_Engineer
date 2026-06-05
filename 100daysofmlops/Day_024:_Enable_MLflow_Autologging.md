## Instruction

The xFusionCorp Industries ML team wanted to eliminate repetitive MLflow logging code such as:

```python
mlflow.log_param(...)
mlflow.log_metric(...)
mlflow.sklearn.log_model(...)
```

and replace it with **MLflow Autologging**.

A training scaffold already existed at:

```bash
/root/code/autolog_experiment.py
```

The script:

* Configured MLflow tracking
* Created a synthetic dataset
* Trained a Scikit-Learn `LogisticRegression`

However, two critical TODO blocks were missing.

The goal was to configure MLflow so that:

* An experiment named `autolog-demo` is automatically created
* A training run is automatically recorded
* All model parameters are captured
* The trained model is logged automatically

# Requirements

After execution:

```bash
python3 /root/code/autolog_experiment.py
```


# Solution

## TODO 1 - Enable Scikit-Learn Autologging

Add:

```python
mlflow.sklearn.autolog()
```

This automatically logs:

* Parameters
* Metrics
* Models
* Training metadata

whenever:

```python
model.fit(...)
```

is executed.

---

## TODO 2 - Set the Active Experiment

Add:

```python
mlflow.set_experiment("autolog-demo")
```

This ensures the run is recorded inside:

```text
autolog-demo
```

instead of:

```text
Default
```

---

# Final Script

```python
import numpy as np
import mlflow
import mlflow.sklearn
from sklearn.linear_model import LogisticRegression

mlflow.set_tracking_uri("http://localhost:5000")

mlflow.sklearn.autolog()

mlflow.set_experiment("autolog-demo")

X = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])
y = np.array([0, 0, 1, 1])

model = LogisticRegression(C=1.0, max_iter=100, random_state=42)
model.fit(X, y)

print("Autolog run complete — check the MLflow UI")
```

<img width="971" height="411" alt="config" src="https://github.com/user-attachments/assets/dfa75cc2-5dc2-43f0-81f6-510f09dc271f" />

# Step-by-Step Execution

## 1. Edit the Script

```bash
vi /root/code/autolog_experiment.py
```

Add the two missing lines.

---

## 2. Execute the Script

```bash
python3 /root/code/autolog_experiment.py
```

Expected output:

```text
Autolog run complete — check the MLflow UI
```

<img width="762" height="292" alt="run" src="https://github.com/user-attachments/assets/5f1e43a9-f069-4ab1-873f-46df7d9591ed" />

## 3. Open MLflow UI

Click:

```text
MLflow UI
```

You should now see:

```text
autolog-demo
```

in the experiment list.

---

## 4. Verify the Run

Open the experiment.

Expected:

```text
Runs: 1
```

(or more if executed multiple times)

---

## 5. Verify Parameters

Under **Parameters**:

```text
C
max_iter
solver
tol
penalty
fit_intercept
random_state
...
```

MLflow automatically logs every constructor parameter.

---

## 6. Verify Artifacts

Under **Artifacts**:

```text
model/
```

Expected contents:

```text
MLmodel
model.pkl
conda.yaml
python_env.yaml
requirements.txt
```

<img width="1745" height="786" alt="mlflow" src="https://github.com/user-attachments/assets/815a581d-faa9-4cde-9345-d725a75d74b8" />


# Fun Message 😄

*"Autologging is basically MLflow saying: ‘Relax, I’ll take notes for you.’ 😄📊"*
