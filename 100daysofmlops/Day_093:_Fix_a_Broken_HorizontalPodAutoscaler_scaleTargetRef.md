# Instruction

The xFusionCorp Industries ML Platform team deployed a `fraud-server` Deployment and configured a HorizontalPodAutoscaler (HPA) to automatically scale it based on CPU utilization.

The HPA was configured with a target CPU utilization of:

```text
70%
```

However, `kubectl get hpa` showed:

```text
TARGETS <unknown>/70%
```

The HPA was unable to determine the current CPU utilization because its `scaleTargetRef` pointed to a Deployment that does not exist.

The objective was to correct the HPA manifest so that it references the actual `fraud-server` Deployment.

# Solution

## Correct `hpa.yaml`

The `scaleTargetRef.name` must point to the existing Deployment:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: fraud-server-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: fraud-server
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

The critical change is:

```yaml
name: fraud-server
```

instead of:

```yaml
name: fraud-serving
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/k8s
```

## Step 2 - Verify the Deployment

```bash
kubectl get deployment fraud-server
```

Expected:

```text
NAME          READY   UP-TO-DATE   AVAILABLE
fraud-server  2/2     2            2
```

✔ The target Deployment exists.

## Step 3 - Inspect the Broken HPA

```bash
kubectl describe hpa fraud-server-hpa
```

The broken reference was:

```text
Reference: Deployment/fraud-serving
```

And the controller reported:

```text
deployments/scale.apps "fraud-serving" not found
```

<img width="812" height="497" alt="hpa before" src="https://github.com/user-attachments/assets/5ae0252d-766e-4139-acbb-95e19648e4c5" />

## Step 4 - Edit the HPA Manifest

```bash
vi hpa.yaml
```

Find:

```yaml
scaleTargetRef:
  apiVersion: apps/v1
  kind: Deployment
  name: fraud-serving
```

Change it to:

```yaml
scaleTargetRef:
  apiVersion: apps/v1
  kind: Deployment
  name: fraud-server
```

## Step 5 - Apply the Corrected Manifest

```bash
kubectl apply -f hpa.yaml
```

Expected:

```text
horizontalpodautoscaler.autoscaling/fraud-server-hpa configured
```

<img width="802" height="267" alt="hpa apply" src="https://github.com/user-attachments/assets/9ec94ac6-5361-4688-9b8e-9fdeda913910" />

<img width="812" height="547" alt="hpa after" src="https://github.com/user-attachments/assets/fd773376-6c4d-45fb-9de4-e606414236dd" />

# HPA Scaling Flow

```text
                    HPA
                     │
                     │ scaleTargetRef
                     ▼
              fraud-server
              Deployment
                     │
             ┌───────┴───────┐
             ▼               ▼
          Pod 1             Pod 2
             │               │
             └───────┬───────┘
                     │
                     ▼
              metrics-server
                     │
                     ▼
              CPU utilization
                     │
                     ▼
                HPA Controller
                     │
             ┌───────┴───────┐
             ▼               ▼
          Scale Up        Scale Down
```
