## Instruction

The xFusionCorp Industries ML Platform team wanted to organize multiple ML projects using separate MLflow Experiments instead of storing everything under the default experiment.

The MLflow Tracking Server was already running and contained:

* `Default` experiment
* `legacy-models` experiment

These experiments served as reference records and **must not be modified**.

The task was to create two new experiments using the MLflow UI and configure their metadata correctly.



# Requirements

## Experiment 1: fraud-detection

Required configuration:

| Property        | Value                             |
| --------------- | --------------------------------- |
| Experiment Name | `fraud-detection`                 |
| Description     | Any non-empty project description |
| Tag Key         | `team`                            |
| Tag Value       | `ml-platform`                     |


## Experiment 2: churn-prediction

Required configuration:

| Property        | Value              |
| --------------- | ------------------ |
| Experiment Name | `churn-prediction` |
| Tag Key         | `team`             |
| Tag Value       | `analytics`        |


# Solution

## ✅ Step 1 - Open the MLflow Dashboard

Click the **MLflow UI** button provided in the lab.

The MLflow dashboard should display:

```text
Default
legacy-models
```

in the experiment list.

<img width="1898" height="327" alt="create" src="https://github.com/user-attachments/assets/9324efd7-6a40-4c39-971c-3728afca377c" />


## Step 2 - Create the fraud-detection Experiment

Click:

```text
Create Experiment
```

Configure:

| Field       | Value                                                    |
| ----------- | -------------------------------------------------------- |
| Name        | `fraud-detection`                                        |
| Description | `Fraud detection project for transaction risk analysis.` |

Save the experiment.



## Step 3 - Add the Team Tag

Open the newly created:

```text
fraud-detection
```

experiment.

Add a tag:

| Key  | Value       |
| ---- | ----------- |
| team | ml-platform |

Save the tag.



## Step 4 - Create the churn-prediction Experiment

Again select:

```text
Create Experiment
```

Configure:

| Field | Value              |
| ----- | ------------------ |
| Name  | `churn-prediction` |

Save the experiment.



## Step 5 - Add the Team Tag

Open:

```text
churn-prediction
```

Add:

| Key  | Value     |
| ---- | --------- |
| team | analytics |

Save.

# Verification ✅

The left sidebar should now contain:

```text
Default
legacy-models
fraud-detection
churn-prediction
```

<img width="1887" height="407" alt="save" src="https://github.com/user-attachments/assets/16bd6975-0e65-4698-8197-e69824d546d7" />


# Fun Message 😄

*"Putting every ML project in the Default experiment is like storing all your files on the desktop — it works until it really doesn’t 😄📊"*
