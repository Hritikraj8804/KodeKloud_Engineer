# Instruction

The Nautilus DevOps team is currently migrating data from on-premise storage to Azure Blob Storage.


* Source file:

```text
/tmp/nautilus.txt
```

* Copy the file into the existing Blob container.

> **Note:** Perform this task using Azure CLI from the `azure-client` host.



# Solution

## **1. Login to Azure**

If not already logged in:

```bash
az login
```

Or retrieve credentials:

```bash
showcreds
```

and login using the provided account.



## **2. Find the Resource Group (Optional)**

```bash
az storage account list --output table
```

Verify the storage account:

```text
nautilusst25874
```

exists.

<img width="1881" height="516" alt="before" src="https://github.com/user-attachments/assets/3746d450-dce7-4368-b2dc-3f07241138a1" />


## **3. Get the Storage Account Key**

```bash
ACCOUNT_KEY=$(az storage account keys list \
  --account-name nautilusst25874 \
  --query "[0].value" \
  -o tsv)
```



## **4. Upload the File to the Blob Container**

```bash
az storage blob upload \
  --account-name nautilusst25874 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-blob-11108 \
  --name nautilus.txt \
  --file /tmp/nautilus.txt
```

<img width="1003" height="497" alt="uploadcli" src="https://github.com/user-attachments/assets/4d6acfa4-d1b4-4b44-99fa-e67138631dba" />


## **5. Verify the Upload**

```bash
az storage blob list \
  --account-name nautilusst25874 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-blob-11108 \
  --output table
```

<img width="1265" height="220" alt="ver" src="https://github.com/user-attachments/assets/b7c06e56-8964-4627-bce8-f60853d6ecfc" />


Expected output should contain:

```text
nautilus.txt
```

<img width="1875" height="471" alt="upload" src="https://github.com/user-attachments/assets/318b68dd-f318-4b38-9496-f06a0f379434" />


# Quick Commands

```bash
ACCOUNT_KEY=$(az storage account keys list \
  --account-name nautilusst25874 \
  --query "[0].value" -o tsv)

az storage blob upload \
  --account-name nautilusst25874 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-blob-11108 \
  --name nautilus.txt \
  --file /tmp/nautilus.txt
```



# Verification

Check that the blob exists:

```bash
az storage blob list \
  --account-name nautilusst25874 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-blob-11108 \
  --output table
```

You should see:

```text
nautilus.txt
```

in the container.

<img width="1265" height="220" alt="ver" src="https://github.com/user-attachments/assets/808cf6ae-f964-4126-86bb-79da4ff7f748" />


## Fun Message

*"The file has successfully sailed from on-prem storage into Azure Blob Storage ☁️📦🚀"*
