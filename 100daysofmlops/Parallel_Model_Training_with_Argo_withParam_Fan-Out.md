# Instruction

The xFusionCorp Industries ML Platform team wants to train multiple **RandomForest model variants in parallel** and then select the best-performing model.

The provided `train-parallel-variants` WorkflowTemplate already contains:

* `train-variant` — trains a model using a configurable `n_estimators`
* `pick-best` — reducer that selects the best model
* `estimators_list` — parameter containing the model variants

However, the `main` template was incomplete.

The objective was to:

* Use Argo's `withParam` to fan out `train-variant`
* Pass each estimator value to the corresponding parallel task
* Add `pick-best` as the fan-in/reducer step
* Apply the WorkflowTemplate
* Submit it once with a faulty value to observe failure isolation
* Submit it again with valid values
* Verify all variants and the reducer succeed

# Solution

## Final `train-parallel-variants.yaml`

```yaml
apiVersion: argoproj.io/v1alpha1
kind: WorkflowTemplate

metadata:
  name: train-parallel-variants
  namespace: argo

spec:
  entrypoint: main

  arguments:
    parameters:
      - name: estimators_list
        value: '["10","50","100"]'

  templates:

    - name: main
      steps:

        - - name: train
            template: train-variant
            arguments:
              parameters:
                - name: n_estimators
                  value: "{{item}}"
            withParam: "{{workflow.parameters.estimators_list}}"

        - - name: pick-best
            template: pick-best

    - name: train-variant
      inputs:
        parameters:
          - name: n_estimators

      script:
        image: alpine:3.19
        command: [sh]
        source: |
          N='{{inputs.parameters.n_estimators}}'

          case "$N" in
            ''|*[!0-9]*)
              echo "[train] ERROR: n_estimators must be a positive integer, got: $N"
              exit 1
              ;;
          esac

          echo "[train] fitting with n_estimators=$N"
          sleep 2
          echo "model-n$N"

    - name: pick-best
      container:
        image: alpine:3.19
        command: [sh, -c]
        args: ["echo '[pick_best] selected: model-n100'"]
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/argo
```

## Step 2 - Edit WorkflowTemplate

```bash
vi train-parallel-variants.yaml
```

Add the `withParam` fan-out:

```yaml
withParam: "{{workflow.parameters.estimators_list}}"
```

and pass each item:

```yaml
arguments:
  parameters:
    - name: n_estimators
      value: "{{item}}"
```

Then add the reducer as a second step group:

```yaml
- - name: pick-best
    template: pick-best
```

## Step 3 - Apply the WorkflowTemplate

```bash
kubectl apply -f train-parallel-variants.yaml -n argo
```

<img width="727" height="92" alt="yml apply" src="https://github.com/user-attachments/assets/22dfc2db-358b-4469-a5f7-bf8b03dcfb89" />

Verify:

```bash
kubectl get workflowtemplate -n argo
```

Expected:

```text
train-parallel-variants
```

<img width="735" height="112" alt="get tempalte" src="https://github.com/user-attachments/assets/9d7c4388-f9b2-44c3-af53-fc5457d1151b" />

# Step 4 - Verify the Template

```bash
kubectl get workflowtemplate train-parallel-variants -n argo -o yaml
```

Confirm the `main` template contains:

```yaml
withParam: "{{workflow.parameters.estimators_list}}"
```

and:

```yaml
- - name: pick-best
    template: pick-best
```

<img width="1381" height="492" alt="get tempalte ui" src="https://github.com/user-attachments/assets/b773218f-61dc-478c-9b53-b01adc1e74b3" />

# Step 5 - Submit a Faulty Run

Open the Argo UI:

```text
http://localhost:5000
```

Navigate to:

```text
Workflow Templates
→ train-parallel-variants
→ Submit
```

<img width="1381" height="492" alt="get tempalte ui" src="https://github.com/user-attachments/assets/cc2c09a8-0325-4732-9969-3c2e89c0d173" />

Override the parameter with a deliberately invalid list, for example:

```json
["10","50","bad"]
```

The resulting execution demonstrates the failure behavior:

```text
train-variant(10)   ✓
train-variant(50)   ✓
train-variant(bad)  ✗
       │
       ▼
  pick-best         Omitted
```

The workflow should finish:

```text
Failed
```

<img width="432" height="275" alt="failed" src="https://github.com/user-attachments/assets/ae1a2180-9216-4894-8142-a6033f495723" />

This demonstrates that the branches are independently executed while the reducer depends on successful completion of the fan-out.

# Step 6 - Submit a Valid Run

Submit the WorkflowTemplate again with:

```json
["10","50","100"]
```

The workflow should create three parallel `train-variant` nodes:

```text
             ┌── train-variant(10)  ✓
             │
estimators ──┼── train-variant(50)  ✓
             │
             └── train-variant(100) ✓
                         │
                         ▼
                     pick-best ✓
```

<img width="355" height="400" alt="success" src="https://github.com/user-attachments/assets/8c0df232-afd9-4daa-97dd-57edac3cfae0" />

# Workflow

```text
WorkflowTemplate
       │
       ▼
estimators_list
       │
       ▼
    withParam
       │
       ├──────────────┐
       ▼              ▼
 train-variant    train-variant
    n=10             n=50
       │              │
       └──────┬───────┘
              │
              ▼
        train-variant
           n=100
              │
              ▼
          pick-best
              │
              ▼
          Succeeded
```

# Fun Message 😄

*"Why train one model when Argo can train three at the same time? Fan-out the experiments, fan-in the winner! 🏎️🤖🏆"*
