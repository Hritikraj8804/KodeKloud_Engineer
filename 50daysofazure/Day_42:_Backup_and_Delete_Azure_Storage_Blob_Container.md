# Instruction

The Nautilus DevOps team needs to back up data from an Azure Blob container before removing it.

### Existing Resources

| Resource        | Value                 |
| --------------- | --------------------- |
| Storage Account | `nautilusst1779`      |
| Container       | `nautilus-blob-24143` |
| Region          | `southcentralus`      |

### Tasks

1. Copy all contents from:

```text
nautilus-blob-24143
```

to:

```text
/opt
```

on the `azure-client` host.

2. Delete the blob container:

```text
nautilus-blob-24143
```

from storage account:

```text
nautilusst1779
```

> **Note:** Use the Azure Portal (UI) to verify resources if needed, but use Azure CLI on the `azure-client` host to download and delete the container.

# Solution

## Step 1: Get Storage Account Access Key

On the `azure-client` host:

Find the resource group:

```bash
az group list -o table
```

Save:

```bash
RG_NAME="<resource-group>"
```

Get the storage account key:

```bash
ACCOUNT_KEY=$(az storage account keys list \
  --account-name nautilusst1779 \
  --resource-group $RG_NAME \
  --query "[0].value" \
  -o tsv)
```

## Step 2: Create Local Backup Directory

```bash
mkdir -p /opt/nautilus-blob-24143
```

## Step 3: Download All Blobs

Copy all files from the container to `/opt`:

```bash
az storage blob download-batch \
  --account-name nautilusst1779 \
  --account-key $ACCOUNT_KEY \
  --source nautilus-blob-24143 \
  --destination /opt/nautilus-blob-24143
```

<img width="756" height="480" alt="downloading" src="https://github.com/user-attachments/assets/37726398-7aa4-4aa3-9dd4-fa77f5679eea" />

Verify:

```bash
ls -l /opt/nautilus-blob-24143
```

<img width="632" height="132" alt="downloaded" src="https://github.com/user-attachments/assets/6a4f8847-be06-4104-aa8e-860809c2a874" />

You should see the downloaded files.

## Step 4: Verify Download

Count downloaded files:

```bash
find /opt/nautilus-blob-24143 -type f
```

Optional:

```bash
du -sh /opt/nautilus-blob-24143
```

Ensure the container contents have been copied before deleting.

## Step 5: Delete the Blob Container

```bash
az storage container delete \
  --name nautilus-blob-24143 \
  --account-name nautilusst1779 \
  --account-key $ACCOUNT_KEY
```

Expected:

```json
{
  "deleted": true
}
```

<img width="602" height="442" alt="deleted" src="https://github.com/user-attachments/assets/0014737d-d450-42f7-9982-ad9143343470" />

## Step 6: Verify Container Deletion

```bash
az storage container list \
  --account-name nautilusst1779 \
  --account-key $ACCOUNT_KEY \
  -o table
```

Verify that:

```text
nautilus-blob-24143
```

is no longer listed.

<img width="1497" height="702" alt="verify ui" src="https://github.com/user-attachments/assets/b2db6fb1-e6f9-442e-8126-aafbc2b15c48" />

