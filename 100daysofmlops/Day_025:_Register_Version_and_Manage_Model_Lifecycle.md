## Instruction

The xFusionCorp Industries ML Platform team wanted to promote trained models from experiment runs into the **MLflow Model Registry** so operations teams could track production and candidate models.

Two runs already existed inside the:

```text
fraud-detection
```

experiment:

| Run      | Hyperparameters                | F1 Score |
| -------- | ------------------------------ | -------- |
| Baseline | n_estimators=100, max_depth=5  | 0.80     |
| Improved | n_estimators=200, max_depth=10 | 0.89     |

The task was to:

* Create a new registered model
* Register both runs as model versions
* Add a model description
* Assign aliases for deployment tracking



# Requirements

## Registered Model

Create:

```text
fraud-detector
```



## Model Description

Add a non-empty description containing the word:

```text
fraud
```

Example:

```text
Fraud detection model for xFusionCorp transaction monitoring.
```



## Version Assignment

Order matters because MLflow assigns versions sequentially.

### Version 1

Source:

```text
Baseline Run
```

Metadata:

```text
Alias = challenger
```



### Version 2

Source:

```text
Improved Run
```

Metadata:

```text
Alias = champion
```



# Solution

## Step 1 - Open MLflow UI

Click:

```text
MLflow UI
```

Open:

```text
fraud-detection
```

experiment.

You should see two runs:

```text
Baseline
Improved
```



# Step 2 - Register the Baseline Run First

Open the baseline run:

```text
n_estimators = 100
max_depth = 5
f1_score = 0.80
```

Navigate to:

```text
Artifacts
→ model
```

Select:

```text
Register Model
```

Create a new model:

```text
fraud-detector
```

Save.

MLflow creates:

```text
fraud-detector v1
```



# Step 3 - Register the Improved Run

Return to:

```text
fraud-detection
```

Open the improved run:

```text
n_estimators = 200
max_depth = 10
f1_score = 0.89
```

Navigate:

```text
Artifacts
→ model
→ Register Model
```

Select:

```text
Existing Model
```

Choose:

```text
fraud-detector
```

Save.

MLflow creates:

```text
fraud-detector v2
```



# Step 4 - Open the Model Registry

Navigate to:

```text
Model Registry
→ fraud-detector
```

You should now see:

```text
Version 1
Version 2
```



# Step 5 - Add Model Description

Edit the model description.

Example:

```text
Fraud detection model for xFusionCorp transaction monitoring and risk scoring.
```

Save.

Requirement satisfied:

```text
Contains the word "fraud"
```



# Step 6 - Assign Alias to Version 1

Open:

```text
Version 1
```

Add alias:

```text
challenger
```

Save.



# Step 7 - Assign Alias to Version 2

Open:

```text
Version 2
```

Add alias:

```text
champion
```

Save.



# Verification ✅

## Model Registry

Expected:

```text
fraud-detector
```

exists.



## Description

Expected:

```text
Fraud detection model ...
```

<img width="1887" height="682" alt="mlflow" src="https://github.com/user-attachments/assets/3b1126f3-a854-47db-9af7-3b9e2f3c57e0" />

## Version Mapping

| Version | Source Run | Alias      |
| ------- | ---------- | ---------- |
| v1      | Baseline   | challenger |
| v2      | Improved   | champion   |


# Fun Message 😄

*"Experiments find good models. The Model Registry remembers which one won the tournament 😄🏆"*
