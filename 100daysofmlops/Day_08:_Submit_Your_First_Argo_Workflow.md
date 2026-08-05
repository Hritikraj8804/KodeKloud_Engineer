# Instruction

The xFusionCorp Industries ML Platform team deployed **Argo Workflows v4.0.4** on a Kubernetes cluster to orchestrate ML pipelines.

The task was to create the **first Argo Workflow** directly from the **Argo UI**, using the **+ Submit New Workflow** form. The workflow needed to:

* Be a valid `Workflow` resource
* Run inside the `argo` namespace
* Use a single container step to simulate model training
* Successfully complete with **Succeeded** status

# Solution Steps

## Step 1 - Open Argo UI

Open the Argo UI:

```text
http://localhost:5000
```

Verify the Workflows page loads successfully.

## Step 2 - Submit New Workflow

Click

```text
+ Submit New Workflow
```

The YAML editor opens.

## Step 3 - Paste Workflow YAML

Replace the existing content with:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: fraud-training-
  namespace: argo

spec:
  entrypoint: train

  templates:
    - name: train
      container:
        image: alpine:3.20
        command:
          - sh
          - -c
        args:
          - echo "Starting fraud model training..." && echo "Training completed successfully!"
```

<img width="1262" height="632" alt="menifest" src="https://github.com/user-attachments/assets/5d4c25d5-8ea5-47e6-82cf-e58e464433dd" />

## Step 4 - Submit

Click

```text
Submit
```

The workflow appears in the Workflows list.

## Step 5 - Monitor Execution

Observe the workflow progressing through:

```text
Pending

↓

Running

↓

Succeeded
```

<img width="1075" height="247" alt="workflow" src="https://github.com/user-attachments/assets/8a80ecff-8000-42c8-9773-ec04502d5a63" />

<img width="1811" height="837" alt="flow" src="https://github.com/user-attachments/assets/30b5a83f-88e4-4569-96b0-9614a801fdbc" />

# Workflow

```text
Argo UI
      │
      ▼
Submit Workflow
      │
      ▼
Workflow Created
      │
      ▼
Container Starts
      │
      ▼
Echo Command Executes
      │
      ▼
Workflow Completes
      │
      ▼
Succeeded
```

# Fun Message 😄

*"Every complex ML pipeline starts with a single workflow. Today's one-step echo becomes tomorrow's automated training, validation, and deployment pipeline! 🚀🤖"*
