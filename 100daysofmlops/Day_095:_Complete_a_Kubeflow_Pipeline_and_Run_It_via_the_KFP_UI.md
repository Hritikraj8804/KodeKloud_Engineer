# Instruction

The xFusionCorp Industries ML Platform team is piloting **Kubeflow Pipelines (KFP)** on a Kind cluster.

A two-component pipeline was provided:

```text
prep_data → train
```

The two `@dsl.component` functions were already implemented, but the `fraud_training_pipeline` function only called `prep_data`.

The objective was to:

* Complete the pipeline DAG
* Make `train` execute after `prep_data`
* Compile the pipeline using the KFP SDK
* Upload the generated IR YAML through the KFP UI
* Create the `fraud-training` pipeline
* Run it from the **Default** experiment
* Verify that the run reaches `SUCCEEDED`


# Solution

## Final `pipeline.py`

```python
"""Two-component Kubeflow Pipelines v2 source.

Structure: ``prep_data`` → ``train``. Each component runs as its own
pod on the kind cluster managed by Kubeflow Pipelines.
"""
from kfp import compiler, dsl

PIPELINE_NAME = "fraud-training"


@dsl.component(base_image="python:3.11-slim")
def prep_data():
    print("[prep_data] synthesising training data: synthetic-rows=100")


@dsl.component(base_image="python:3.11-slim")
def train():
    print("[train] training on synthetic data -> model artefact ready")


@dsl.pipeline(
    name=PIPELINE_NAME,
    description="Synthetic two-step training pipeline for the KFP lab.",
)
def fraud_training_pipeline():
    prep = prep_data()

    train().after(prep)


if __name__ == "__main__":
    compiler.Compiler().compile(
        pipeline_func=fraud_training_pipeline,
        package_path="pipeline.yaml",
    )
    print("Wrote pipeline.yaml -- upload this file via the KFP UI.")
```


# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/kfp
```


## Step 2 - Edit Pipeline

```bash
vi pipeline.py
```

Complete the pipeline function:

```python
def fraud_training_pipeline():
    prep = prep_data()
    train().after(prep)
```

The important part is:

```python
train().after(prep)
```

This creates the dependency:

```text
prep_data → train
```


## Step 3 - Compile the Pipeline

Run:

```bash
python3 pipeline.py
```

Expected output:

```text
Wrote pipeline.yaml -- upload this file via the KFP UI.
```

<img width="722" height="578" alt="pipeline yaml" src="https://github.com/user-attachments/assets/053c8276-9c27-41d8-aa05-d0c87a1dae20" />

This generates:

```text
/root/code/kfp/pipeline.yaml
```


# Kubeflow Pipeline Compilation

The source:

```text
pipeline.py
```

is compiled into:

```text
pipeline.yaml
```

The workflow is:

```text
@dsl.component
       │
       ▼
@dsl.pipeline
       │
       ▼
KFP Compiler
       │
       ▼
pipeline.yaml
       │
       ▼
KFP UI
       │
       ▼
Pipeline
       │
       ▼
Pipeline Run
```


# Step 4 - Open KFP UI

Open the **KFP UI** button in the lab.

The UI should be reachable at:

```text
http://localhost:5000
```

<img width="1882" height="465" alt="pipeline ui" src="https://github.com/user-attachments/assets/6b2d5c53-ecfe-45c3-abf3-a26d15d79014" />

Verify that the KFP interface loads successfully.


# Step 5 - Upload Pipeline

In the KFP UI:

1. Open **Pipelines**
2. Select **Upload pipeline**
3. Choose **Upload a file**
4. Select the locally downloaded `pipeline.yaml`
5. Set the pipeline name to:

```text
fraud-training
```

<img width="840" height="842" alt="new pipeline" src="https://github.com/user-attachments/assets/78bcf88d-046f-4f86-a742-2125a2a3dd08" />

6. Upload it.

The pipeline should now appear in the pipeline list.


# Step 6 - Create a Run

Open:

```text
fraud-training
```

Create a new run.

Select the:

```text
Default
```

experiment.

<img width="1260" height="787" alt="run" src="https://github.com/user-attachments/assets/f80d8a7b-7505-450c-b9be-bfaf3566379c" />

Submit the run.


<img width="742" height="415" alt="success" src="https://github.com/user-attachments/assets/ac26507d-f2f5-4fd4-bcb9-d9e093e933cc" />

# KFP DAG Flow

```text
                 Pipeline
                    │
                    ▼
              fraud-training
                    │
                    ▼
                prep_data
                    │
                    │ .after(prep)
                    ▼
                  train
                    │
                    ▼
              Pipeline Run
                    │
                    ▼
                SUCCEEDED
```


# Source → Pipeline → Run

```text
pipeline.py
     │
     │ KFP Compiler
     ▼
pipeline.yaml
     │
     │ Upload
     ▼
KFP Pipeline
fraud-training
     │
     │ Create Run
     ▼
Default Experiment
     │
     ▼
┌──────────────┐
│   prep_data  │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│    train     │
└──────┬───────┘
       │
       ▼
   SUCCEEDED
```



# Fun Message 😄

*"A pipeline isn't just a collection of steps—it is the dependency graph between them. `prep_data()` creates the work, but `.after(prep)` tells KFP when `train()` is actually allowed to start. 🚀"*
