# Instruction

The xFusionCorp Industries ML Platform team created a three-stage Argo Workflow to simulate an ML training pipeline:

```text
data-prep → train → evaluate
```

Each step runs in its own pod and shares a workspace volume.

However, the workflow was failing because the **evaluate** task started immediately instead of waiting for the **train** task to finish writing `model.pkl`.

The objective was to:

* Investigate the failed DAG in the Argo UI.
* Fix the dependency in the Workflow YAML.
* Submit the corrected workflow through the **Argo UI YAML editor**.
* Verify that the workflow completes successfully with **Succeeded** status.


# Solution Steps

## Step 1 - Open Argo UI

Open:

```text
http://localhost:5000
```

## Step 2 - Inspect Failed Workflow

Open the failed workflow:

```text
training-pipeline-xxxxx
```

Click the red **evaluate** node.

Observe the logs:

```text
[evaluate] checking model

[evaluate] ERROR: model.pkl not found
```

## Step 3 - Submit New Workflow

Click:

```text
+ Submit New Workflow
```

## Step 4 - Paste Corrected YAML

Paste the corrected workflow with:

```yaml
dependencies:
  - train
```
under the **evaluate** task.


### Final YAML

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Workflow
metadata:
  generateName: training-pipeline-
  namespace: argo
  labels:
    lab: day86

spec:
  entrypoint: main

  volumeClaimTemplates:
    - metadata:
        name: workdir
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 64Mi

  templates:
    - name: main
      dag:
        tasks:
          - name: data-prep
            template: data-prep

          - name: train
            template: train
            dependencies:
              - data-prep

          - name: evaluate
            template: evaluate
            dependencies:
              - train

    - name: data-prep
      container:
        image: alpine:3.19
        command: [sh, -c]
        args:
          - |
            echo "[data-prep] preparing data"
            sleep 2
            echo "rows=100" > /workdir/data.txt
            echo "[data-prep] done"
        volumeMounts:
          - name: workdir
            mountPath: /workdir

    - name: train
      container:
        image: alpine:3.19
        command: [sh, -c]
        args:
          - |
            echo "[train] training model"
            sleep 5
            echo "model-v1" > /workdir/model.pkl
            echo "[train] done"
        volumeMounts:
          - name: workdir
            mountPath: /workdir

    - name: evaluate
      container:
        image: alpine:3.19
        command: [sh, -c]
        args:
          - |
            echo "[evaluate] checking model"
            if [ ! -f /workdir/model.pkl ]; then
              echo "[evaluate] ERROR: model.pkl not found"
              exit 1
            fi
            cat /workdir/model.pkl
            echo "[evaluate] done"
        volumeMounts:
          - name: workdir
            mountPath: /workdir
```
## Step 5 - Submit

Click

```text
Submit
```

## Step 6 - Observe DAG

The DAG should now execute as:

```text
data-prep

↓

train

↓

evaluate
```

All three nodes become green.

<img width="1386" height="812" alt="workflow" src="https://github.com/user-attachments/assets/c4449811-d62e-4d44-a677-c6e1efeb07ae" />

# Workflow

```text
Submit Workflow
        │
        ▼
data-prep
        │
        ▼
Generate data.txt
        │
        ▼
train
        │
        ▼
Generate model.pkl
        │
        ▼
evaluate
        │
        ▼
Read model.pkl
        │
        ▼
Succeeded
```

# Fun Message 😄

*"A DAG isn't just about connecting tasks—it's about telling Argo **when** each task is allowed to begin. One missing dependency can turn a perfectly good pipeline into a race condition! 🏁⚙️"*
