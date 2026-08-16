# Instruction

The xFusionCorp Industries ML Platform team is adopting **GitOps** for Kubernetes workloads using ArgoCD.

ArgoCD was already running on the `mlops` Kind cluster, and the objective was to create an Application named:

```text
guestbook
```

The Application needed to track the canonical ArgoCD example repository:

```text
https://github.com/argoproj/argocd-example-apps
```

using the:

```text
guestbook
```

path and deploy the manifests into the Kubernetes `default` namespace.

Automatic synchronization was also required so that ArgoCD continuously reconciles the cluster against Git.


# Solution Steps

## Step 1 - Open ArgoCD

Open the **ArgoCD UI** button provided by the lab.

The UI is available on:

```text
http://localhost:5000
```

Log in with:

```text
Username: admin
Password: admin
```

## Step 2 - Create Application

From the ArgoCD UI:

```text
Applications
      ↓
New App
```

Create an Application with:

```text
Application Name: guestbook
Project: default
```

## Step 3 - Configure Source

Set the repository to:

```text
https://github.com/argoproj/argocd-example-apps
```

Set:

```text
Revision: HEAD
Path: guestbook
```

This tells ArgoCD to use:

```text
argoproj/argocd-example-apps
└── guestbook/
```

as the source of truth.

## Step 4 - Configure Destination

Set the cluster destination to:

```text
https://kubernetes.default.svc
```

Namespace:

```text
default
```

## Step 5 - Enable Automatic Sync

Enable automated synchronization.

The resulting configuration is conceptually:

```yaml
syncPolicy:
  automated:
    prune: true
    selfHeal: true
```

Automatic sync allows ArgoCD to reconcile the cluster without requiring the operator to manually press **Sync**.

## Step 6 - Create the Application

Click:

```text
Create
```

ArgoCD should begin processing the repository.

The Application initially goes through states such as:

```text
OutOfSync
    ↓
Syncing
    ↓
Synced
```

Health should eventually become:

```text
Healthy
```

# GitOps Reconciliation Flow 🔄

```text
        Git Repository
              │
              │
              ▼
     argoproj/argocd-example-apps
              │
              │ guestbook/
              ▼
          ┌────────┐
          │ ArgoCD │
          └────┬───┘
               │
        Automatic Sync
               │
               ▼
      Kubernetes Cluster
               │
               ▼
        default namespace
               │
               ▼
        Guestbook workload
```

# Fun Message 😄

*"Git says what should exist, Kubernetes says what actually exists, and ArgoCD is the slightly obsessive friend who keeps making them match. 🔄☸️"*
