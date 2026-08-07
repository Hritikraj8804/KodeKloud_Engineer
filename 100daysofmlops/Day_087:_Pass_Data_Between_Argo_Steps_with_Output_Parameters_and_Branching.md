# Instruction

The xFusionCorp Industries ML Platform team wanted a reusable **Argo WorkflowTemplate** that trains, evaluates, and only registers a model if it passes a configurable quality gate.

The provided `train-and-maybe-register.yaml` already contained the training pipeline, but two important pieces were missing:

* Publish the evaluation score as an **output parameter**.
* Add a **when:** condition so the `register` step only executes when the evaluation score is greater than or equal to the configurable `min_score`.

Finally, the template needed to be applied and executed twice from the **Argo UI**:

* **Run 1:** `min_score = 0.99` → Register step should be **Skipped**
* **Run 2:** `min_score = 0.5` → Register step should **Run Successfully**


# Solution

## TODO 1 — Publish the Evaluation Score

Inside the **evaluate** template, add:

```yaml
outputs:
  parameters:
    - name: score
      valueFrom:
        path: /tmp/score.txt
```

## TODO 2 — Add the Quality Gate

Modify the **register** step:

```yaml
- - name: register
    template: register
    when: "{{steps.evaluate.outputs.parameters.score}} >= {{workflow.parameters.min_score}}"
```

# Final WorkflowTemplate

```yaml
apiVersion: argoproj.io/v1alpha1
kind: WorkflowTemplate

metadata:
  name: train-and-maybe-register
  namespace: argo

spec:
  entrypoint: main

  arguments:
    parameters:
      - name: min_score
        value: "0.80"

  templates:

    - name: main
      steps:
        - - name: train
            template: train

        - - name: evaluate
            template: evaluate

        - - name: register
            template: register
            when: "{{steps.evaluate.outputs.parameters.score}} >= {{workflow.parameters.min_score}}"

    - name: train
      container:
        image: alpine:3.19
        command: [sh, -c]
        args:
          - echo "[train] fitting model" && sleep 2

    - name: evaluate
      script:
        image: alpine:3.19
        command: [sh]
        source: |
          echo "0.75" > /tmp/score.txt
          echo "[evaluate] score=0.75"

      outputs:
        parameters:
          - name: score
            valueFrom:
              path: /tmp/score.txt

    - name: register
      container:
        image: alpine:3.19
        command: [sh, -c]
        args:
          - echo "[register] promoting model to registry"
```

# Solution Steps

## Step 1 - Edit WorkflowTemplate

```bash
cd /root/code/argo

vi train-and-maybe-register.yaml
```

Add:

```yaml
outputs:
  parameters:
    - name: score
      valueFrom:
        path: /tmp/score.txt
```

and

```yaml
when: "{{steps.evaluate.outputs.parameters.score}} >= {{workflow.parameters.min_score}}"
```

## Step 2 - Apply the Template

```bash
kubectl apply -f train-and-maybe-register.yaml
```

Expected:

```text
workflowtemplate.argoproj.io/train-and-maybe-register created
```

or

```text
configured
```

<img width="772" height="92" alt="apply" src="https://github.com/user-attachments/assets/b0534b7b-e907-4b24-b86a-93834a7112bc" />

## Step 3 - Open Argo UI

Open

```text
http://localhost:5000
```

Go to

```text
Workflow Templates
```

Verify:

```text
train-and-maybe-register
```

appears.

## Step 4 - Submit First Workflow

Click

```text
Submit
```

Set

```text
min_score

0.99
```

Run the workflow.

<img width="757" height="292" alt="argo submit" src="https://github.com/user-attachments/assets/194506fd-9112-4c03-adfe-90ab66048036" />

Expected:

```text
train

↓

evaluate

↓

register (Skipped)
```

Workflow status:

```text
Succeeded
```

<img width="1285" height="690" alt="ui" src="https://github.com/user-attachments/assets/a3478893-bee5-486b-8322-3ba461913e09" />

## Step 5 - Submit Second Workflow

Submit the template again.

Use

```text
min_score

0.5
```

Expected:

```text
train

↓

evaluate

↓

register
```

Register node:

```text
Succeeded
```

<img width="756" height="287" alt="argo submit with" src="https://github.com/user-attachments/assets/15eeb754-248a-4a15-9fcf-0b50c85664e9" />

Workflow:

```text
Succeeded
```

<img width="1217" height="727" alt="ui with" src="https://github.com/user-attachments/assets/4084f5f0-a0a8-4012-92a7-3016b406c111" />

# Workflow

```text
Train
   │
   ▼
Evaluate
(score = 0.75)
   │
   ▼
Compare with min_score
   │
   ├──────── False ───────► Register Skipped
   │
   └──────── True ────────► Register Executed
```

# Fun Message 😄

*"Train every model, evaluate every model—but only promote the champions. That's the essence of automated MLOps quality gates! 🏆🤖"*
