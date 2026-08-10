# Instruction

The xFusionCorp Industries ML Platform team requires the `fraud-detector` model to be retrained automatically on a fixed schedule.

A `CronWorkflow` scaffold was provided at:

```text
/root/code/argo/fraud-retraining.yaml
```

The scaffold was incomplete in two places:

* The `schedules` cron expression was missing.
* The retraining container still contained a placeholder command that exited with status `1`.

The objective was to:

* Schedule retraining every minute
* Add a successful retraining command
* Apply the CronWorkflow to the `argo` namespace
* Verify that Argo activates the CronWorkflow
* Confirm that Argo automatically creates a child Workflow
* Verify the scheduled Workflow reaches `Succeeded`

# Solution

## Final `fraud-retraining.yaml`

```yaml
apiVersion: argoproj.io/v1alpha1
kind: CronWorkflow

metadata:
  name: fraud-retraining
  namespace: argo

spec:
  schedules:
    - "* * * * *"

  timezone: "Etc/UTC"
  concurrencyPolicy: Forbid
  startingDeadlineSeconds: 60
  successfulJobsHistoryLimit: 4
  failedJobsHistoryLimit: 2

  workflowSpec:
    entrypoint: main

    templates:
      - name: main
        container:
          image: alpine:3.19
          command: [sh, -c]
          args:
            - |
              echo "retrain: $(date -u)"
              exit 0
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/argo
```

## Step 2 - Edit CronWorkflow

```bash
vi fraud-retraining.yaml
```

Add the schedule:

```yaml
schedules:
  - "* * * * *"
```

The cron expression:

```text
* * * * *
```

means:

```text
minute hour day month weekday
```

with every field matching every value, resulting in an execution every minute.

## Step 3 - Add Retraining Command

Replace the placeholder:

```yaml
args:
  - |
    echo "TODO 2: author the retraining step"
    exit 1
```

with:

```yaml
args:
  - |
    echo "retrain: $(date -u)"
    exit 0
```

The command simulates model retraining while returning a successful exit status.

# Step 4 - Apply CronWorkflow

```bash
kubectl apply -f fraud-retraining.yaml -n argo
```

Expected:

```text
cronworkflow.argoproj.io/fraud-retraining created
```

<img width="715" height="81" alt="code apply" src="https://github.com/user-attachments/assets/2c5ad114-e49d-4c80-8b52-b11f4ad31826" />

# Step 5 - Verify CronWorkflow

```bash
kubectl get cronworkflow fraud-retraining -n argo
```

Expected:

```text
NAME               SCHEDULE      SUSPEND   ACTIVE   LAST RUN
fraud-retraining   * * * * *     False
```

The important properties are:

```text
SCHEDULE → * * * * *
SUSPEND  → False
```

✔

# Step 6 - Check CronWorkflow Details

```bash
kubectl get cronworkflow fraud-retraining -n argo -o yaml
```

Verify:

```yaml
spec:
  schedules:
    - "* * * * *"
```

and:

```yaml
spec:
  concurrencyPolicy: Forbid
```

✔

# Step 7 - Open Argo UI

Open:

```text
http://localhost:5000
```

Navigate to:

```text
Cron Workflows
```

The CronWorkflow should appear as:

```text
fraud-retraining
```

It should be active and should not show a `Suspended` badge.

# Step 8 - Wait for Scheduled Run

Because the schedule is every minute, Argo automatically creates a child Workflow.

Check from the terminal:

```bash
kubectl get workflows -n argo
```

Expected:

```text
NAME                     STATUS
fraud-retraining-xxxxx   Succeeded
```

The generated Workflow will also carry the CronWorkflow ownership label.

<img width="656" height="140" alt="task cli" src="https://github.com/user-attachments/assets/3f7d8de4-925a-467e-85a1-74b3555e4a71" />

<img width="1005" height="226" alt="tasks ui" src="https://github.com/user-attachments/assets/c05e096d-0ee0-4ba1-b85f-bc70d1c8ef5e" />

# Workflow

```text
                  Cron Schedule
                  * * * * *
                       │
                       ▼
              fraud-retraining
                 CronWorkflow
                       │
                       ▼
              Child Workflow
                       │
                       ▼
                  main template
                       │
                       ▼
              Retraining container
                       │
                       ▼
            echo "retrain: timestamp"
                       │
                       ▼
                    exit 0
                       │
                       ▼
                  Succeeded
```

# Fun Message 😄

*"Why wait for someone to click Submit? Let the cron wake up the model and retrain it while everyone is sleeping. ⏰🤖🌙"*
