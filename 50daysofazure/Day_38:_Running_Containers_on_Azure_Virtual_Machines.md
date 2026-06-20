# Instruction

The Nautilus DevOps team needs to configure Azure Blob Storage and upload a file from an existing VM.

### Existing Resource

| Resource | Name         |
| -------- | ------------ |
| VM       | `xfusion-vm` |
| Region   | `East US`    |

# Solution

## 1. Get Resource Group

```bash
az group list -o table
```

Example:

```bash
RG_NAME="kml_rg_main-xxxx"
```

<img width="510" height="121" alt="rs grp" src="https://github.com/user-attachments/assets/3338bd3b-d19e-43b7-9e03-82d8c0b26f16" />

## 2. Create Storage Account

```bash
az storage account create \
  --name xfusionstor12152 \
  --resource-group $RG_NAME \
  --location eastus \
  --sku Standard_LRS
```

Verify:

```bash
az storage account show \
  --name xfusionstor12152 \
  --resource-group $RG_NAME \
  --query "{name:name,location:location,sku:sku.name}" \
  -o table
```

Expected:

```text
Name               Location   Sku
-----------------  ---------  ------------
xfusionstor12152   eastus     Standard_LRS
```

## 3. Get Storage Account Key

```bash
ACCOUNT_KEY=$(az storage account keys list \
  --resource-group $RG_NAME \
  --account-name xfusionstor12152 \
  --query "[0].value" \
  -o tsv)
```

Verify:

```bash
echo $ACCOUNT_KEY
```

<img width="867" height="196" alt="storage key" src="https://github.com/user-attachments/assets/ad787749-4446-44dc-b804-595a97c75ace" />

## 4. Create Private Blob Container

Private is the default access level.

```bash
az storage container create \
  --name xfusion-container12152 \
  --account-name xfusionstor12152 \
  --account-key $ACCOUNT_KEY
```

<img width="566" height="407" alt="storage acc create" src="https://github.com/user-attachments/assets/217049a9-4df6-4103-88c9-60dc477ba3ca" />

Verify:

```bash
az storage container show \
  --name xfusion-container12152 \
  --account-name xfusionstor12152 \
  --account-key $ACCOUNT_KEY
```

Look for:

```text
publicAccess : null
```

which confirms the container is private.

<img width="502" height="207" alt="storage container" src="https://github.com/user-attachments/assets/caf0eac3-20b9-4f97-9476-44ef0dd8b1c1" />

# Create File on VM

## 5. Get VM Public IP

```bash
az vm show \
  --name xfusion-vm \
  --resource-group $RG_NAME \
  -d \
  --query publicIps \
  -o tsv
```

Example:

```text
20.x.x.x
```

<img width="537" height="215" alt="vm ip" src="https://github.com/user-attachments/assets/f522063b-b112-4fa1-b64d-91c41cf67673" />

## 6. SSH Into VM

```bash
ssh azureuser@<VM_PUBLIC_IP>
```

<img width="840" height="282" alt="vm ssh" src="https://github.com/user-attachments/assets/96877938-f144-439a-a5be-8cc39490e307" />

## 7. Create Test File

```bash
echo "this is a test file" > /home/azureuser/testfile.txt
```

Verify:

```bash
cat /home/azureuser/testfile.txt
```

Expected:

```text
this is a test file
```

<img width="745" height="122" alt="tset file" src="https://github.com/user-attachments/assets/12590d2c-14dc-40d0-863e-36aceebdcc9b" />

# Upload File to Blob Storage

## 8. Upload Blob

From inside the VM:

```bash
az storage blob upload \
  --account-name xfusionstor12152 \
  --account-key <ACCESS_KEY> \
  --container-name xfusion-container12152 \
  --name testfile.txt \
  --file /home/azureuser/testfile.txt
```

Replace:

```text
<ACCESS_KEY>
```

with the value from Step 3.

Example:

```bash
az storage blob upload \
  --account-name xfusionstor12152 \
  --account-key $ACCOUNT_KEY \
  --container-name xfusion-container12152 \
  --name testfile.txt \
  --file /home/azureuser/testfile.txt
```

Expected:

```json
{
  "etag": "...",
  "lastModified": "..."
}
```

<img width="762" height="232" alt="file upload" src="https://github.com/user-attachments/assets/53460709-ad63-435d-8977-4c6c2be66546" />

## 9. Verify Upload

```bash
az storage blob list \
  --account-name xfusionstor12152 \
  --account-key $ACCOUNT_KEY \
  --container-name xfusion-container12152 \
  -o table
```

Expected:

```text
testfile.txt
```

<img width="731" height="276" alt="storage blob list" src="https://github.com/user-attachments/assets/6c17fa07-e877-4d71-8a6e-f142f84e811f" />

## Fun Message

*"The VM and Blob Storage are now connected, and your test file has successfully landed in Azure Storage 🚀☁️📦"*
