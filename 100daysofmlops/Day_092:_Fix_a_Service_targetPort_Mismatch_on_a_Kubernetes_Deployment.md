# Instruction

The xFusionCorp Industries ML Platform team deployed a `fraud-detector` Kubernetes Deployment with two replicas.

The pods are running an `nginx:alpine` container that listens on:

```text
80
```

However, clients are expected to access the application through:

```text
fraud-detector-svc:8080
```

The Service was configured with:

```text
port: 8080
targetPort: 8080
```

This caused the Service to forward traffic to port `8080` on the pods, where nothing was listening.

The objective was to correct the Service so that:

```text
Client → Service :8080 → Pod :80
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/k8s
```

## Step 2 - Inspect the Deployment

```bash
kubectl get deploy fraud-detector
```

Expected:

```text
NAME             READY   UP-TO-DATE   AVAILABLE
fraud-detector   2/2     2            2
```

<img width="667" height="90" alt="deploy get" src="https://github.com/user-attachments/assets/bef04d76-eef2-4a17-bb3c-0fa8b4377c60" />

✔ The Deployment is healthy.

## Step 3 - Inspect the Service

```bash
kubectl describe svc fraud-detector-svc
```

The broken configuration showed:

```text
Port:        8080/TCP
TargetPort:  8080/TCP
Endpoints:   10.244.0.5:8080,10.244.0.6:8080
```

<img width="707" height="477" alt="svc desc" src="https://github.com/user-attachments/assets/58c4a1e1-0077-4e5d-8d25-54dbe13b30a9" />

The Service had endpoints, but they pointed to the wrong container port.

## Step 4 - Edit the Service Manifest

```bash
vi service.yaml
```

Change:

```yaml
targetPort: 8080
```

to:

```yaml
targetPort: 80
```

Keep:

```yaml
port: 8080
```

because clients must continue using:

```text
fraud-detector-svc:8080
```

## Step 5 - Apply the Corrected Service

```bash
kubectl apply -f service.yaml
```

Expected:

```text
service/fraud-detector-svc configured
```

<img width="732" height="92" alt="svc apply" src="https://github.com/user-attachments/assets/c4b2e55a-4fde-4ed1-bccd-d294ed663be4" />

<img width="717" height="447" alt="svc desc2" src="https://github.com/user-attachments/assets/d3cc0faf-d43b-4ef7-b6ad-901daa511210" />

<img width="721" height="397" alt="curl" src="https://github.com/user-attachments/assets/f8e16bc7-623c-410e-9848-8e849366f18c" />

# Final Architecture 🎯

```text
                    Client
                      │
                      │ HTTP :8080
                      ▼
            ┌─────────────────────┐
            │ fraud-detector-svc  │
            │     port: 8080      │
            └──────────┬──────────┘
                       │
                 targetPort: 80
                       │
             ┌─────────┴─────────┐
             ▼                   ▼
      ┌─────────────┐     ┌─────────────┐
      │ fraud Pod 1 │     │ fraud Pod 2 │
      │  nginx :80  │     │  nginx :80  │
      └─────────────┘     └─────────────┘
```

# Fun Message 😄

*"The pods were healthy, the Service existed, and Kubernetes was routing exactly as configured—the problem was simply that it was knocking on port 8080 while nginx was answering on port 80. 🔌☸️"*
