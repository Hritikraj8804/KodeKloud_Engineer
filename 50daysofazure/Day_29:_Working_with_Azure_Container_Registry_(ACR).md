# Instruction

The Nautilus DevOps team needs to create an Azure Container Registry (ACR), build a Docker image from an existing Dockerfile, and push the image to the registry.

### Requirements

| Resource            | Value                    |
| ------------------- | ------------------------ |
| ACR Name            | `devopsacr31042`         |
| Region              | `East US`                |
| SKU                 | `Basic`                  |
| Dockerfile Location | `/root/pyapp/Dockerfile` |
| Image Tag           | `devopsacr31042:latest`  |

> **Note:** Create the ACR using the Azure Portal (UI), then use the `azure-client` terminal to build and push the image.

# Part 1: Create Azure Container Registry (UI)

## Step 1: Login to Azure Portal

Open:

```text
https://portal.azure.com
```

Login using the provided Azure credentials.

## Step 2: Open Container Registries

Search:

```text
Container Registries
```

Click:

```text
+ Create
```

## Step 3: Configure ACR

### Basics

| Setting        | Value                       |
| -------------- | --------------------------- |
| Resource Group | Existing lab resource group |
| Registry Name  | `devopsacr31042`            |
| Region         | `East US`                   |
| SKU            | `Basic`                     |

Click:

```text
Review + Create
```

Then:

```text
Create
```

Wait until deployment completes.

<img width="627" height="802" alt="docker register review" src="https://github.com/user-attachments/assets/3cb475e5-76ef-4a55-a3d9-b953d9576e9e" />

# Part 2: Enable Admin User

Open:

```text
devopsacr31042
```

Navigate:

```text
Settings → Access Keys
```

Enable:

```text
Admin User = Enabled
```

Click:

```text
Save
```

Copy:

```text
Login Server
Username
Password
```

Example:

```text
devopsacr31042.azurecr.io
```

<img width="1312" height="572" alt="access key" src="https://github.com/user-attachments/assets/2afa9b4e-2bf7-42e9-a4ed-d829740715c0" />

# Part 3: Build the Docker Image

## Step 4: Login to azure-client

Go to the terminal on the `azure-client` host.

## Step 5: Move to Application Directory

```bash
cd /root/pyapp
```

Verify:

```bash
ls
```

Expected:

```text
Dockerfile
```

<img width="581" height="90" alt="files" src="https://github.com/user-attachments/assets/0f77c6ef-f846-4eff-b586-c0a2e9b32ab3" />

## Step 6: Login to Azure

```bash
az login
```

If already logged in, continue.


## Step 7: Login to ACR

Using Azure CLI:

```bash
az acr login --name devopsacr31042
```

Expected:

```text
Login Succeeded
```

<img width="722" height="180" alt="docker login" src="https://github.com/user-attachments/assets/b3de2a17-4c2a-4b21-aee0-f69aeab94490" />

## Step 8: Build the Image

Build using the Dockerfile:

```bash
docker build -t devopsacr31042:latest .
```

Verify:

```bash
docker images
```

Expected:

```text
devopsacr31042   latest
```

<img width="1027" height="581" alt="docker build" src="https://github.com/user-attachments/assets/498c0447-1a33-4509-9a0e-d1c25b7c37ba" />

# Part 4: Tag and Push the Image

## Step 9: Tag the Image

```bash
docker tag devopsacr31042:latest \
devopsacr31042.azurecr.io/devopsacr31042:latest
```

## Step 10: Push to ACR

```bash
docker push \
devopsacr31042.azurecr.io/devopsacr31042:latest
```

Wait for the push to complete.

Expected:

```text
latest: digest: sha256:...
```

<img width="780" height="167" alt="tag and push" src="https://github.com/user-attachments/assets/55874b9e-f0df-4ad5-9636-4610a4a7382f" />

# Part 5: Verify the Repository

## Step 11: Verify Using Azure CLI

```bash
az acr repository list \
  --name devopsacr31042 \
  --output table
```

Expected:

```text
devopsacr31042
```


## Step 12: Verify the Tag

```bash
az acr repository show-tags \
  --name devopsacr31042 \
  --repository devopsacr31042 \
  --output table
```

Expected:

```text
latest
```

<img width="1711" height="617" alt="verify" src="https://github.com/user-attachments/assets/802fde1c-3690-4555-b39d-50d137433595" />
