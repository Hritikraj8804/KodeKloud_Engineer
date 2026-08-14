# Instruction

The xFusionCorp Industries ML Platform team deployed a **KServe InferenceService** named `fraud-detector` on a Kind Kubernetes cluster.

The predictor uses a Scikit-learn runtime and loads its model from a Kubernetes PersistentVolumeClaim (PVC).

The InferenceService was not reaching the **Ready** state because the predictor pod remained in `Pending`.

The objective was to diagnose the scheduling failure, correct the KServe manifest, and verify that the predictor can successfully serve a prediction.

# Solution

The `storageUri` must reference the **existing PVC** in the same namespace.

First, identify the available PVC:

```bash
kubectl get pvc
```

Use the actual model-storage PVC name returned by this command.

Then update `/root/code/k8s/inference-service.yaml`.

For example, if the existing PVC is named `model-storage`, the predictor section should contain:

```yaml
spec:
  predictor:
    model:
      modelFormat:
        name: sklearn
      storageUri: pvc://model-storage/
```

The important part is:

```yaml
storageUri: pvc://<existing-pvc>/
```

instead of:

```yaml
storageUri: pvc://models-storage/
```

# Solution Steps

## Step 1 - Check Existing PVCs

```bash
kubectl get pvc
```

Identify the PVC containing the pre-staged model.

<img width="702" height="117" alt="pvc" src="https://github.com/user-attachments/assets/e0fb9405-d941-4471-a3a9-1f306ecd3373" />

The PVC must exist in the same namespace as:

```text
fraud-detector
```

## Step 2 - Inspect the InferenceService

```bash
kubectl get isvc fraud-detector -o yaml
```

Check:

```yaml
spec:
  predictor:
    model:
      storageUri:
```

The current value is pointing to:

```text
pvc://models-storage/
```

## Step 3 - Edit the Manifest

```bash
vi /root/code/k8s/inference-service.yaml
```

Change the incorrect PVC reference to the PVC that actually exists.

For example:

```yaml
spec:
  predictor:
    model:
      modelFormat:
        name: sklearn
      storageUri: pvc://model-storage/
```

Do not change the model itself.

## Step 4 - Apply the Corrected Manifest

```bash
kubectl apply -f /root/code/k8s/inference-service.yaml
```

Expected:

```text
inferenceservice.serving.kserve.io/fraud-detector configured
```

<img width="710" height="90" alt="apply" src="https://github.com/user-attachments/assets/e72ddf44-c806-4433-bdc2-512981eb1d81" />

KServe should recreate/reconcile the predictor deployment.

# Verification

## Verify PVC

```bash
kubectl get pvc
```

Expected:

```text
STATUS
Bound
```

✔ The referenced PVC exists.

## Verify Predictor Pod

```bash
kubectl get pods -l serving.kserve.io/inferenceservice=fraud-detector
```

Initially it may show:

```text
Pending
```

Then it should progress to:

```text
Running
```

For example:

```text
NAME                                      READY   STATUS
fraud-detector-predictor-xxxxxxxxxx       1/1     Running
```

<img width="722" height="282" alt="check" src="https://github.com/user-attachments/assets/cade78b4-50d6-47b5-95ab-4f6e0edecd37" />

✔

## Verify InferenceService

```bash
kubectl get isvc fraud-detector
```

The service should eventually report Ready:

```text
NAME             READY
fraud-detector   True
```

Or inspect the conditions directly:

```bash
kubectl get isvc fraud-detector -o jsonpath='{.status.conditions[?(@.type=="Ready")].status}'
```

Expected:

```text
True
```

✔

## Verify Predictor Model Mount

```bash
kubectl describe pod -l serving.kserve.io/inferenceservice=fraud-detector
```

The predictor should now be scheduled onto the node and the model directory should be available at:

```text
/mnt/models
```

The KServe container uses:

```text
--model_dir=/mnt/models
```

✔

# Test Prediction

Once the InferenceService becomes Ready, send a prediction request to the predictor endpoint.

The request follows the KServe V1 prediction API:

```bash
curl -X POST \
  http://<predictor-endpoint>/v1/models/fraud-detector:predict \
  -H 'Content-Type: application/json' \
  -d '{
    "instances": [
      [3200, 23, 5]
    ]
  }'
```

Expected response shape:

```json
{
  "predictions": [
    1
  ]
}
```

The exact prediction can depend on the pre-trained model, but the important requirement is that the response contains:

```json
"predictions": [...]
```

<img width="612" height="237" alt="prediction" src="https://github.com/user-attachments/assets/e9729836-ee59-4df1-8b13-7340ae96ee00" />

✔

# KServe Serving Flow

```text
                 Kubernetes
                     │
                     ▼
          ┌─────────────────────┐
          │ InferenceService    │
          │ fraud-detector      │
          └──────────┬──────────┘
                     │
                     ▼
              Predictor Pod
                     │
                     │ mounts
                     ▼
              Existing PVC
                     │
                     ▼
                /mnt/models
                     │
                     ▼
          sklearnserver runtime
                     │
                     ▼
        /v1/models/fraud-detector
                     │
                     ▼
              predictions[]
```

# Fun Message 😄

*"KServe wasn't broken—the model wasn't even getting a seat on the Kubernetes bus. 🚌☸️ Fix the PVC reference, and the predictor can finally get scheduled!"*
