# Instruction

The xFusionCorp Industries MLOps team is implementing a GitOps deployment workflow for the `fraud-detector` model server.

The deployment architecture uses:

* **Gitea** as the Git source of truth
* **ArgoCD** as the GitOps reconciliation engine
* **Kubernetes** as the deployment platform
* **nginx** as the model-server stand-in

The objective was to:

1. Complete the ArgoCD `Application` manifest
2. Point it at the `mlops-deploy` Gitea repository
3. Deploy the Kubernetes manifests from the `manifests` directory
4. Sync the application through ArgoCD
5. Update the image from `nginx:1.25-alpine` to `nginx:1.27-alpine` in Gitea
6. Sync again and verify the rollout

# Solution

## Final `application.yaml`

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application

metadata:
  name: fraud-detector
  namespace: argocd

spec:
  project: default

  source:
    repoURL: http://gitea-http.gitea.svc.cluster.local:3000/gitops-admin/mlops-deploy.git
    targetRevision: HEAD
    path: manifests

  destination:
    server: https://kubernetes.default.svc
    namespace: default
```

# Solution Steps

## Step 1 - Navigate to the Project

```bash
cd /root/code
```

## Step 2 - Edit the ArgoCD Application

```bash
vi /root/code/application.yaml
```

Fill the three TODO values.

### Repository URL

```yaml
repoURL: http://gitea-http.gitea.svc.cluster.local:3000/gitops-admin/mlops-deploy.git
```

### Manifest Path

```yaml
path: manifests
```

### Destination Namespace

```yaml
namespace: default
```

## Step 3 - Apply the Application

```bash
kubectl apply -n argocd -f /root/code/application.yaml
```

Expected:

```text
application.argoproj.io/fraud-detector created
```

<img width="753" height="131" alt="apply" src="https://github.com/user-attachments/assets/53efd169-7130-45b2-8175-7162c126ab90" />

# ArgoCD UI

Open the **ArgoCD UI** on port `5000`.

Log in with:

```text
Username: admin
Password: adminadmin
```

Navigate to:

```text
Applications
└── fraud-detector
```

The application should eventually show:

```text
Sync:   Synced
Health: Healthy
```

<img width="1600" height="653" alt="sync before" src="https://github.com/user-attachments/assets/07b8f901-667c-4f4a-8d5a-f4413956398f" />

<img width="513" height="272" alt="spec before" src="https://github.com/user-attachments/assets/c368bd58-4477-4379-86e0-5daa54409f40" />

# Step 4 - Verify the Application

```bash
kubectl get application fraud-detector -n argocd
```

Expected state:

```text
NAME             SYNC STATUS   HEALTH STATUS
fraud-detector   Synced        Healthy
```

# Step 5 - Verify the Initial Deployment

```bash
kubectl get deployment fraud-detector
```

Check the image:

```bash
kubectl get deployment fraud-detector \
  -o jsonpath='{.spec.template.spec.containers[0].image}'
```

Initially:

```text
nginx:1.25-alpine
```

The Service exposes the application through:

```text
localhost:8085
```

Verify:

```bash
curl -i http://localhost:8085/
```

Expected:

```text
HTTP/1.1 200 OK
```

# Step 6 - Update the Image in Gitea

The **Gitea repository is the source of truth**.

Open the Gitea UI on port `3000` and log in:

```text
Username: gitops-admin
Password: adminadmin
```

Open:

```text
gitops-admin/mlops-deploy
```

Navigate to:

```text
manifests/deployment.yaml
```

Change:

```yaml
image: nginx:1.25-alpine
```

to:

```yaml
image: nginx:1.27-alpine
```

<img width="658" height="737" alt="gitea update" src="https://github.com/user-attachments/assets/de6d1456-edc3-4633-a687-e7435d8defb2" />

Commit the change directly through the **Gitea web editor**.

# Step 7 - Sync the New Version

Return to the ArgoCD UI.

Open:

```text
Applications
└── fraud-detector
```

ArgoCD should detect the Git change.

Click:

```text
Sync
```

and synchronize the application.

The Deployment should roll out the new image.

<img width="1440" height="258" alt="out of sync" src="https://github.com/user-attachments/assets/c5643e27-6f28-4dec-93b2-7f7c686f071e" />

<img width="1598" height="671" alt="sync after" src="https://github.com/user-attachments/assets/1f5ee21e-9c69-4576-9800-2c820c901575" />

<img width="526" height="253" alt="spec after" src="https://github.com/user-attachments/assets/26c6ec06-3b95-45d7-85ea-1dc7e5bc9e15" />

<img width="740" height="363" alt="check" src="https://github.com/user-attachments/assets/b7e1c949-caec-47a1-9575-231c743008cc" />

# Fun Message 😄

*"Don't deploy the cluster—commit the desired state. Git tells ArgoCD what should exist, and ArgoCD makes Kubernetes agree. 🚀☸️"*
