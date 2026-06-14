# Instruction

As part of the data migration project, you need to create a new private Blob container and migrate a file between containers using Azure CLI.

### Requirements

| Resource              | Value                   |
| --------------------- | ----------------------- |
| Storage Account       | `nautilusst17067`       |
| Source Container      | `nautilus-source-32388` |
| Destination Container | `nautilus-dest-28457`   |
| File                  | `nautilus.txt`          |

### Tasks

1. Create a **private** Blob container named:

```text
nautilus-dest-28457
```

2. Copy:

```text
nautilus.txt
```

from:

```text
nautilus-source-32388
```

to:

```text
nautilus-dest-28457
```

3. Ensure the file exists in **both** containers.
4. Verify the contents are identical.


# Solution

## 1. Login to Azure

```bash
az login
```


## 2. Get Storage Account Key

```bash
ACCOUNT_KEY=$(az storage account keys list \
  --account-name nautilusst17067 \
  --query "[0].value" \
  -o tsv)
```


## 3. Create the Destination Container

Create it as private (default behavior):

```bash
az storage container create \
  --name nautilus-dest-28457 \
  --account-name nautilusst17067 \
  --account-key $ACCOUNT_KEY
```

<img width="652" height="470" alt="blob create" src="https://github.com/user-attachments/assets/4f6f1684-9a34-4bc1-a5d5-8ceda18f20f8" />

Verify:

```bash
az storage container show \
  --name nautilus-dest-28457 \
  --account-name nautilusst17067 \
  --account-key $ACCOUNT_KEY
```

Look for:

```text
publicAccess : null
```

which means private.

<img width="400" height="187" alt="blob create verify" src="https://github.com/user-attachments/assets/d1674aae-07b3-4a9f-b05b-700368c5525d" />


## 4. Download the Source File

```bash
az storage blob download \
  --account-name nautilusst17067 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-source-32388 \
  --name nautilus.txt \
  --file /tmp/nautilus.txt
```


## 5. Upload the File to Destination Container

```bash
az storage blob upload \
  --account-name nautilusst17067 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-dest-28457 \
  --name nautilus.txt \
  --file /tmp/nautilus.txt \
  --overwrite
```


## 6. Verify File Exists in Source Container

```bash
az storage blob list \
  --account-name nautilusst17067 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-source-32388 \
  --output table
```

Expected:

```text
nautilus.txt
```


## 7. Verify File Exists in Destination Container

```bash
az storage blob list \
  --account-name nautilusst17067 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-dest-28457 \
  --output table
```

Expected:

```text
nautilus.txt
```


## 8. Verify Content Integrity

Download the destination copy:

```bash
az storage blob download \
  --account-name nautilusst17067 \
  --account-key $ACCOUNT_KEY \
  --container-name nautilus-dest-28457 \
  --name nautilus.txt \
  --file /tmp/nautilus-dest.txt
```

Compare both files:

```bash
diff /tmp/nautilus.txt /tmp/nautilus-dest.txt
```

Expected:

```text
(no output)
```

No output means the contents are identical.

<img width="442" height="60" alt="diff" src="https://github.com/user-attachments/assets/75baeb41-40ca-4fd1-9be9-36f40fac869e" />

🚀 Data migration completed successfully with integrity preserved.
