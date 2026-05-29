## Instruction

The xFusionCorp Industries Data Science team uses **DVC Experiments** to compare different training runs without modifying the main workspace.

The `fraud-detection` project already had:

* A parameterized DVC pipeline ✅
* `params.yaml` with:

```yaml
n_estimators: 100
```

* A baseline model and metrics already generated ✅

The task was to:

* Run multiple experiments with different `n_estimators` values
* Compare their performance
* Identify the experiment with the highest `f1_score`
* Promote the best experiment to the tracked workspace



# Requirements

Run three experiments with different values:

```yaml
n_estimators: 50
n_estimators: 200
n_estimators: 500
```

Each experiment should:

* Train a new model
* Generate a new `metrics.json`
* Record metrics and parameters in DVC

The experiment with the highest:

```json
{
  "f1_score": ...
}
```

must be applied to the workspace.



# Solution

## ✅ Step 1 - Navigate to the Project

```bash
cd /root/code/fraud-detection
```



# Step 2 - Run Experiment 1

Using 50 estimators:

```bash
dvc exp run -S n_estimators=50
```

Expected output:

```text
Reproduced experiment(s): exp-xxxx
```

<img width="1003" height="338" alt="e_50" src="https://github.com/user-attachments/assets/4b88f399-9881-40da-9f97-f1a73ff80a7c" />


# Step 3 - Run Experiment 2

Using 200 estimators:

```bash
dvc exp run -S n_estimators=200
```

<img width="1027" height="342" alt="e_200" src="https://github.com/user-attachments/assets/35599661-78b0-45e3-8c4b-1bb418cc0238" />


# Step 4 - Run Experiment 3

Using 500 estimators:

```bash
dvc exp run -S n_estimators=500
```

<img width="948" height="341" alt="e_500" src="https://github.com/user-attachments/assets/87439b13-6807-40d5-a7a6-176c35bba693" />


# Step 5 - List All Experiments

Display experiment results:

```bash
dvc exp show
```

Example output:

```text
┏━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━┳━━━━━━━━━━━┳━━━━━━━━━━┓
┃ Experiment   ┃ n_estimators ┃ accuracy  ┃ f1_score ┃
┡━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━╇━━━━━━━━━━━╇━━━━━━━━━━┩
│ baseline     │ 100          │ 0.91      │ 0.89     │
│ exp-1234     │ 50           │ 0.90      │ 0.88     │
│ exp-5678     │ 200          │ 0.94      │ 0.92     │
│ exp-9012     │ 500          │ 0.93      │ 0.91     │
└──────────────┴──────────────┴───────────┴──────────┘
```

*(Actual values depend on the dataset and training script.)*

<img width="897" height="611" alt="exp_show" src="https://github.com/user-attachments/assets/f39183f9-6b13-4e15-9c69-f5ac41feffdf" />


# Step 6 - Identify the Best Experiment

Compare the `f1_score` column.

Example:

| Experiment | n_estimators | f1_score   |
| ---------- | ------------ | ---------- |
| Baseline   | 100          | 0.89       |
| Exp 1      | 50           | 0.88       |
| Exp 2      | 200          | **0.92** ✅ |
| Exp 3      | 500          | 0.91       |

Best experiment:

```text
n_estimators = 200
```

because it achieved the highest `f1_score`.



# Step 7 - Apply the Best Experiment

Apply the chosen experiment:

```bash
dvc exp apply exp-5678
```

Replace:

```bash
exp-5678
```

with the actual experiment name from your output.

<img width="976" height="285" alt="exp_apply" src="https://github.com/user-attachments/assets/4f948ec0-dff9-4928-9754-65c6656fd0ba" />


# Step 8 - Verify params.yaml

Check:

```bash
cat params.yaml
```

Expected:

```yaml
n_estimators: 200
```



# Step 9 - Verify Metrics

```bash
dvc metrics show
```

Expected:

```text
Path          accuracy    f1_score
metrics.json  0.94        0.92
```



# Step 10 - Verify Model Regeneration

```bash
ls -l models/model.pkl
```

The model now corresponds to the selected experiment.



# Understanding the Concept 💡

## What are DVC Experiments?

DVC Experiments allow you to:

* Change parameters
* Run training
* Compare results

without creating Git commits.

Think of them as lightweight experiment branches.



## Why Use Experiments?

Instead of:

```text
Edit params.yaml
Train
Commit
Repeat...
```

You can simply run:

```bash
dvc exp run -S n_estimators=200
```

and DVC tracks everything automatically.



## What Does `dvc exp show` Do?

Displays:

* Parameters
* Metrics
* Experiment names

in a single comparison table.



## What Does `dvc exp apply` Do?

Promotes an experiment into the current workspace.

This updates:

* `params.yaml`
* `metrics.json`
* `model.pkl`

to match the selected experiment.


# Fun Message 😄

*"Training one model tells you if it works. Training ten models tells you if it’s actually good 😄🧪"*

