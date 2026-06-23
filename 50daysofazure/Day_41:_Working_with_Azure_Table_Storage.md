# Instruction

The Nautilus DevOps team needs to create an Azure Table Storage solution for a simple To-Do application.

### Requirements

#### Storage Account

| Setting         | Value                    |
| --------------- | ------------------------ |
| Storage Account | `datacentertablest30477` |
| Region          | `East US`                |

#### Table

| Setting    | Value   |
| ---------- | ------- |
| Table Name | `tasks` |

#### Task Records

**Task 1**

| Property     | Value               |
| ------------ | ------------------- |
| PartitionKey | tasks               |
| RowKey       | 1                   |
| description  | Learn Table Storage |
| status       | completed           |

**Task 2**

| Property     | Value           |
| ------------ | --------------- |
| PartitionKey | tasks           |
| RowKey       | 2               |
| description  | Build To-Do App |
| status       | in-progress     |

> **Note:** Azure Portal does not provide a convenient UI to insert Table entities. Create the Storage Account using the Portal, then use Azure Cloud Shell or the azure-client terminal to create the table and insert entities.

# Part 1: Create Storage Account (UI)

## Step 1: Login to Azure Portal

Open:

```text
https://portal.azure.com
```

## Step 2: Create Storage Account

Search:

```text
Storage Accounts
```

Click:

```text
+ Create
```

Configure:

| Setting              | Value                           |
| -------------------- | ------------------------------- |
| Resource Group       | Existing Lab Resource Group     |
| Storage Account Name | `datacentertablest30477`        |
| Region               | `East US`                       |
| Performance          | Standard                        |
| Redundancy           | Locally-redundant storage (LRS) |

Click:

```text
Review + Create
```

<img width="1041" height="767" alt="create storage acc" src="https://github.com/user-attachments/assets/43d3a6a6-304e-4930-bfa5-70638ec662bc" />

Then:

```text
Create
```

Wait until deployment completes.

# Part 2: Open Azure Cloud Shell

After the Storage Account is created:

Click:

```text
>_ Cloud Shell
```

at the top of the Azure Portal.

Select:

```text
Bash
```

if prompted.

# Part 3: Create the Table

## Step 3: Get Storage Account Key

```bash
RG_NAME=$(az group list --query "[0].name" -o tsv)

ACCOUNT_KEY=$(az storage account keys list \
  --resource-group $RG_NAME \
  --account-name datacentertablest30477 \
  --query "[0].value" \
  -o tsv)
```

## Step 4: Create Table

If needed, install the extension:

```bash
az extension add --name storage-preview
```

Create the table:

```bash
az storage table create \
  --name tasks \
  --account-name datacentertablest30477 \
  --account-key $ACCOUNT_KEY
```

<img width="1675" height="756" alt="tables" src="https://github.com/user-attachments/assets/28c29805-ddb1-4795-9129-2104e77a382b" />

# Part 4: Insert Task 1

```bash
az storage entity insert \
  --table-name tasks \
  --account-name datacentertablest30477 \
  --account-key $ACCOUNT_KEY \
  --entity \
    PartitionKey=tasks \
    RowKey=1 \
    description="Learn Table Storage" \
    status="completed"
```

<img width="997" height="172" alt="instert 1" src="https://github.com/user-attachments/assets/56828be9-60f0-431b-9476-41304032088d" />

# Part 5: Insert Task 2

```bash
az storage entity insert \
  --table-name tasks \
  --account-name datacentertablest30477 \
  --account-key $ACCOUNT_KEY \
  --entity \
    PartitionKey=tasks \
    RowKey=2 \
    description="Build To-Do App" \
    status="in-progress"
```

<img width="881" height="230" alt="instert 2" src="https://github.com/user-attachments/assets/dca575db-72e9-4d01-b671-e5875a7352af" />

# Part 6: Verify Task 1

```bash
az storage entity show \
  --table-name tasks \
  --account-name datacentertablest30477 \
  --account-key $ACCOUNT_KEY \
  --partition-key tasks \
  --row-key 1
```

Expected:

```json
{
  "description": "Learn Table Storage",
  "status": "completed"
}
```

# Part 7: Verify Task 2

```bash
az storage entity show \
  --table-name tasks \
  --account-name datacentertablest30477 \
  --account-key $ACCOUNT_KEY \
  --partition-key tasks \
  --row-key 2
```

Expected:

```json
{
  "description": "Build To-Do App",
  "status": "in-progress"
}
```

<img width="632" height="630" alt="verify cli" src="https://github.com/user-attachments/assets/c787a0a3-10a7-4a36-88cc-1e0499c24ebb" />

<img width="1765" height="581" alt="verify ui" src="https://github.com/user-attachments/assets/740ce89c-7e19-4893-9332-63b7ababa7a3" />

## Fun Message

*"Your Azure Table Storage-powered To-Do backend is now live, with tasks neatly stored and ready for your application 🚀📋☁️"*
