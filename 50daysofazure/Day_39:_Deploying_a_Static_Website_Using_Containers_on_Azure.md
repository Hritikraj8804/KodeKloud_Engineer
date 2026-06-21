# Instruction

The Nautilus DevOps team needs to host a static website using Azure Storage and make it publicly accessible.

### Requirements

| Resource        | Value                |
| --------------- | -------------------- |
| Storage Account | `nautiluswebst14873` |
| Region          | `East US`            |
| Static Website  | Enabled              |
| Index Document  | `index.html`         |
| File Source     | `/root/index.html`   |
| Container       | `$web`               |
| Access          | Public               |

> **Note:** Use the Azure Portal (UI) to create and configure the Storage Account. Use Azure CLI to upload the file.

# Solution

## Part 1: Create the Storage Account (UI)

### 1. Login to Azure Portal

Open:

```text
https://portal.azure.com
```

### 2. Create Storage Account

Search:

```text
Storage Accounts
```

Click:

```text
+ Create
```

Configure:

| Setting              | Value                       |
| -------------------- | --------------------------- |
| Resource Group       | Existing Lab Resource Group |
| Storage Account Name | `nautiluswebst14873`        |
| Region               | `East US`                   |
| Performance          | Standard                    |
| Redundancy           | LRS                         |

Click:

```text
Review + Create
```

Then:

```text
Create
```

<img width="1082" height="575" alt="storage create" src="https://github.com/user-attachments/assets/fd4b8a3c-5d54-4d59-be7b-e189d0039690" />

## Part 2: Enable Static Website Hosting (UI)

Open:

```text
nautiluswebst14873
```

Navigate:

```text
Data Management → Static Website
```

Click:

```text
Enabled
```

Configure:

| Setting             | Value        |
| ------------------- | ------------ |
| Index Document Name | `index.html` |
| Error Document Path | Leave Blank  |

Click:

```text
Save
```

<img width="972" height="762" alt="static website enable" src="https://github.com/user-attachments/assets/80590c5d-cfd6-4d74-87df-41be6aad1817" />

Azure automatically creates:

```text
$web
```

container.

## Part 3: Allow Public Access (UI)

Navigate:

```text
Settings → Configuration
```

Ensure:

```text
Allow Blob Anonymous Access = Enabled
```

Click:

```text
Save
```

> In many Azure Portal versions, Static Website hosting automatically exposes content publicly through the Static Website endpoint.

# Part 4: Upload index.html Using CLI

## 4. Get Storage Account Key

On the `azure-client` host:

```bash
RG_NAME=$(az group list --query "[0].name" -o tsv)

ACCOUNT_KEY=$(az storage account keys list \
  --resource-group $RG_NAME \
  --account-name nautiluswebst14873 \
  --query "[0].value" \
  -o tsv)
```

## 5. Verify File Exists

```bash
ls -l /root/index.html
```

## 6. Upload File to $web Container

```bash
az storage blob upload \
  --account-name nautiluswebst14873 \
  --account-key $ACCOUNT_KEY \
  --container-name '$web' \
  --name index.html \
  --file /root/index.html \
  --overwrite
```

Expected:

````text
Finished[#############################################################] 100.0000%
````

<img width="842" height="497" alt="file upload" src="https://github.com/user-attachments/assets/5e6b9085-d0b1-4890-938f-cf6a2fb3a305" />

<img width="1081" height="497" alt="file upload ui" src="https://github.com/user-attachments/assets/a97c5630-1ad1-40ae-8a1a-4b055d94cc6c" />

## Part 5: Verify Website URL

### 7. Get Static Website Endpoint

```bash
az storage account show \
  --name nautiluswebst14873 \
  --resource-group $RG_NAME \
  --query "primaryEndpoints.web" \
  -o tsv
````

Example:

```text
https://nautiluswebst14873.z13.web.core.windows.net/
```

<img width="682" height="196" alt="website" src="https://github.com/user-attachments/assets/9416f7eb-a973-4514-b0d4-ebf88ebb84b7" />

### 8. Test the Website

```bash
curl $(az storage account show \
  --name nautiluswebst14873 \
  --resource-group $RG_NAME \
  --query "primaryEndpoints.web" \
  -o tsv)
```

Or open the URL in a browser.

You should see the contents of:

```text
/root/index.html
```


## Fun Message

*"Your static website is now live on Azure Storage and can be accessed globally without a web server 🚀🌐☁️"*
