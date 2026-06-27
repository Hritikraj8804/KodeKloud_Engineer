# Instruction

The Nautilus DevOps team needs to create a private Azure Kubernetes Service (AKS) cluster for deploying Kubernetes workloads.

### Requirements

#### AKS Cluster

| Setting            | Value             |
| ------------------ | ----------------- |
| Cluster Name       | `nautilus-aks`    |
| Region             | `Central US`      |
| Kubernetes Version | `1.33.0`          |
| API Server Access  | Private           |
| Availability       | High Availability |
| Monitoring         | Disabled          |

#### Node Pool

| Setting     | Value             |
| ----------- | ----------------- |
| VM Size     | `Standard_D2s_v3` |
| Min Nodes   | `1`               |
| Max Nodes   | `2`               |
| Autoscaling | Enabled           |

> **Note:** Use the Azure Portal (UI) to create and configure the AKS cluster.

# Solution

## Part 1: Create the AKS Cluster

### Step 1: Login to Azure Portal

Open:

```text
https://portal.azure.com
```

### Step 2: Navigate to AKS

Search:

```text
Kubernetes services
```

Click:

```text
+ Create
→ Kubernetes cluster
```

# Basics

Configure:

| Setting            | Value                                    |
| ------------------ | ---------------------------------------- |
| Resource Group     | Existing Lab Resource Group              |
| Cluster Name       | `nautilus-aks`                           |
| Region             | `Central US`                             |
| Kubernetes Version | `1.33.0`                                 |
| Availability Zones | Default (or all available if selectable) |

Authentication:

Leave defaults.

Click:

```text
Next : Node pools
```

# Part 2: Configure Node Pool

The default system node pool is created automatically.

Configure it as follows:

| Setting    | Value               |
| ---------- | ------------------- |
| Pool Name  | nodepool1 (default) |
| Mode       | System              |
| VM Size    | `Standard_D2s_v3`   |
| Node Count | 1                   |

Enable:

```text
Cluster Autoscaler
```

Configure:

| Setting       | Value |
| ------------- | ----- |
| Minimum Nodes | 1     |
| Maximum Nodes | 2     |

> **Important:** If the wizard creates additional node pools, remove them after the cluster is created so that only the required node pool remains.

Click:

```text
Next : Access
```

# Part 3: Configure Private Cluster

Under **Access** (or **Networking**, depending on the Azure Portal version):

Locate:

```text
API Server Access
```

Select:

```text
Private
```

or enable:

```text
Private Cluster
```

This ensures the Kubernetes API endpoint is private.

# Part 4: Disable Monitoring

Navigate to:

```text
Integrations
```

or

```text
Monitoring
```

Disable:

```text
Container Insights
```

Disable:

```text
Azure Monitor
```

Disable any other monitoring options presented.

Continue.

# Part 5: Networking

Leave networking defaults unless the lab specifies otherwise.

Click:

```text
Review + Create
```

<img width="1062" height="540" alt="k8s review" src="https://github.com/user-attachments/assets/af2401ba-4e81-477c-9b74-bb9cde0294e2" />

Then:

```text
Create
```

> **Note:** AKS deployment may take **10–15 minutes**.

Wait until the cluster status becomes:

```text
Running
```

# Part 6: Verify Node Pools

After deployment:

Open:

```text
nautilus-aks
```

Navigate:

```text
Node Pools
```

If additional node pools exist:

Select the extra node pool.

Click:

```text
Delete
```

Keep only the required system node pool.

Verify:

| Setting   | Value           |
| --------- | --------------- |
| VM Size   | Standard_D2s_v3 |
| Min Nodes | 1               |
| Max Nodes | 2               |

<img width="1756" height="857" alt="ui" src="https://github.com/user-attachments/assets/205103e2-a967-4011-a0ee-ad209721581c" />

# Part 7: Verify Private Cluster

Open:

```text
Overview
```

or

```text
Properties
```

Verify:

```text
Private Cluster = Yes
```

or

```text
API Server Access = Private
```

# Part 8: Verify Monitoring

Navigate to:

```text
Insights
```

or

```text
Monitoring
```

Verify:

```text
Container Insights
```

is:

```text
Disabled
```

No Azure Monitor integrations should be enabled.

<img width="1001" height="172" alt="aks cli verify" src="https://github.com/user-attachments/assets/c16648e5-8268-4b76-a856-3543310818a0" />

## Fun Message

*"Your private AKS cluster is now provisioned with autoscaling, private API access, and a streamlined configuration, ready to host Kubernetes workloads securely 🚀☸️☁️"*
