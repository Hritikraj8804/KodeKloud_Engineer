# Instruction

As part of the data migration process, the Nautilus DevOps team is actively creating several storage containers on Azure. They plan to utilize **public Blob containers** to store relevant data.

For this task:

1. Create a Storage Account named:

```text
devopsst1914
```

2. Create a Blob Container named:

```text
devops-blob-26469
```

3. Enable anonymous read access.
4. Configure the container for **public access to both containers and blobs**.

> **Note:** Perform all operations using the Azure CLI from the `azure-client` host.



# Solution

## **1. Login to Azure**

```bash
az login
```



## **2. Find the Resource Group**

If the resource group name is not provided:

```bash
az group list --output table
```

Note the existing resource group name and use it in the commands below.



## **3. Create the Storage Account**

```bash
az storage account create \
    --name devopsst1914 \
    --resource-group <RESOURCE_GROUP> \
    --location eastus \
    --sku Standard_LRS \
    --allow-blob-public-access true
```

Replace:

```text
<RESOURCE_GROUP>
```

with the actual resource group name.

<img width="647" height="733" alt="storage" src="https://github.com/user-attachments/assets/d3e62e37-470d-4564-84ee-612b347e1ca3" />


## **4. Get the Storage Account Key**

```bash
ACCOUNT_KEY=$(az storage account keys list \
    --resource-group <RESOURCE_GROUP> \
    --account-name devopsst1914 \
    --query "[0].value" \
    -o tsv)
```



## **5. Create the Public Blob Container**

```bash
az storage container create \
    --name devops-blob-26469 \
    --account-name devopsst1914 \
    --account-key $ACCOUNT_KEY \
    --public-access container
```

The option:

```bash
--public-access container
```

provides anonymous read access for both:

* Containers
* Blobs

which is exactly what the task requires.

<img width="502" height="220" alt="container" src="https://github.com/user-attachments/assets/679c585d-6ca6-42f2-8b3f-edff6fcdbb55" />


## **6. Verify the Container**

```bash
az storage container show \
    --name devops-blob-26469 \
    --account-name devopsst1914 \
    --account-key $ACCOUNT_KEY \
    --output table
```

Look for:

```text
publicAccess
container
```

<img width="585" height="346" alt="verify" src="https://github.com/user-attachments/assets/d0d77e0c-4ba0-4d41-8b24-38a6e88c28a7" />


# Configuration Summary

| Resource            | Value               |
| ------------------- | ------------------- |
| Storage Account     | `devopsst1914`      |
| Container           | `devops-blob-26469` |
| SKU                 | `Standard_LRS`      |
| Anonymous Access    | Enabled             |
| Public Access Level | `container`         |



# Quick Commands

```bash
az storage account create \
  --name devopsst1914 \
  --resource-group <RESOURCE_GROUP> \
  --location eastus \
  --sku Standard_LRS \
  --allow-blob-public-access true

ACCOUNT_KEY=$(az storage account keys list \
  --resource-group <RESOURCE_GROUP> \
  --account-name devopsst1914 \
  --query "[0].value" -o tsv)

az storage container create \
  --name devops-blob-26469 \
  --account-name devopsst1914 \
  --account-key $ACCOUNT_KEY \
  --public-access container
```



## Fun Message

*"Your Azure blob container is now open to the world and ready to serve data from the cloud ☁️📦🌍"*
