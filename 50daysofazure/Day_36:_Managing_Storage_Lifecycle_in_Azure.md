# Instruction

The Nautilus DevOps team needs to implement **Azure Blob Lifecycle Management** to automatically delete old blobs and reduce storage costs.

### Requirements

#### Storage Account

| Setting    | Value             |
| ---------- | ----------------- |
| Name       | `devopsstor28216` |
| Region     | `East US`         |
| Redundancy | `LRS`             |

#### Blob Container

| Setting | Value                   |
| ------- | ----------------------- |
| Name    | `devops-container28216` |

#### File

| Setting | Value                |
| ------- | -------------------- |
| File    | `/root/tempfile.txt` |

#### Lifecycle Management Rule

| Setting   | Value                          |
| --------- | ------------------------------ |
| Rule Name | `devops-del-rule`              |
| Action    | Delete Blob                    |
| Condition | 7 Days After Last Modification |

> **Note:** This solution uses Azure CLI and works well in KodeKloud Azure labs.

# Solution

## 1. Find the Resource Group

```bash
az group list -o table
```

Example:

```bash
RG_NAME="kml_rg_main-xxxxxxxx"
```

<img width="457" height="191" alt="variables" src="https://github.com/user-attachments/assets/96de60c0-9ae5-415e-8c05-0b8e5b9d7313" />

## 2. Create the Storage Account

```bash
az storage account create \
  --name devopsstor28216 \
  --resource-group $RG_NAME \
  --location eastus \
  --sku Standard_LRS
```

<img width="852" height="702" alt="create storage acc" src="https://github.com/user-attachments/assets/938fa040-2fed-47e4-bb53-01c6318e3598" />

Verify:

```bash
az storage account show \
  --name devopsstor28216 \
  --resource-group $RG_NAME \
  --query "{name:name,location:location,sku:sku.name}" \
  -o table
```

## 3. Get Storage Account Key

```bash
ACCOUNT_KEY=$(az storage account keys list \
  --resource-group $RG_NAME \
  --account-name devopsstor28216 \
  --query "[0].value" \
  -o tsv)
```

<img width="426" height="77" alt="acc key" src="https://github.com/user-attachments/assets/31d10b8d-a920-4cfc-901e-6dcba7964953" />

## 4. Create the Blob Container

```bash
az storage container create \
  --name devops-container28216 \
  --account-name devopsstor28216 \
  --account-key $ACCOUNT_KEY
```

<img width="852" height="702" alt="create storage acc" src="https://github.com/user-attachments/assets/73206b96-c716-4029-bdc2-a18f35792983" />

Verify:

```bash
az storage container list \
  --account-name devopsstor28216 \
  --account-key $ACCOUNT_KEY \
  -o table
```

<img width="437" height="456" alt="show storage acc" src="https://github.com/user-attachments/assets/4a5a5fb0-3180-4568-b11c-40e820ceebd6" />

## 5. Upload tempfile.txt

Verify file exists:

```bash
ls -l /root/tempfile.txt
```

Upload:

```bash
az storage blob upload \
  --account-name devopsstor28216 \
  --account-key $ACCOUNT_KEY \
  --container-name devops-container28216 \
  --name tempfile.txt \
  --file /root/tempfile.txt
```

<img width="632" height="342" alt="storage blob" src="https://github.com/user-attachments/assets/e785a012-e63c-415b-8041-958f386ca73b" />

Verify:

```bash
az storage blob list \
  --account-name devopsstor28216 \
  --account-key $ACCOUNT_KEY \
  --container-name devops-container28216 \
  -o table
```

Expected:

```text
tempfile.txt
```

<img width="747" height="177" alt="show blob" src="https://github.com/user-attachments/assets/5d4680b4-d5ba-4292-a079-f80449169961" />

## 6. Create Lifecycle Management Policy File

Create:

```bash
vi lifecycle.json
```

Paste:

```json
{
  "rules": [
    {
      "enabled": true,
      "name": "devops-del-rule",
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": [
            "blockBlob"
          ],
          "prefixMatch": [
            "devops-container28216"
          ]
        },
        "actions": {
          "baseBlob": {
            "delete": {
              "daysAfterModificationGreaterThan": 7
            }
          }
        }
      }
    }
  ]
}
```

Save the file.

## 7. Apply Lifecycle Management Policy

```bash
az storage account management-policy create \
  --account-name devopsstor28216 \
  --resource-group $RG_NAME \
  --policy @lifecycle.json
```

<img width="755" height="615" alt="management policies" src="https://github.com/user-attachments/assets/1d7cf4d5-fea2-4001-aee5-260148e3a6ff" />

## 8. Verify Lifecycle Rule

```bash
az storage account management-policy show \
  --account-name devopsstor28216 \
  --resource-group $RG_NAME
```

Look for:

```json
"name": "devops-del-rule"
```

and

```json
"daysAfterModificationGreaterThan": 7
```

<img width="652" height="532" alt="management policies show" src="https://github.com/user-attachments/assets/2318cc11-264e-41b2-a02a-85cc0cebeb01" />

## Fun Message

*"Your Azure storage now cleans up old blobs automatically, keeping costs low and storage tidy 🚀🗑️☁️"*
