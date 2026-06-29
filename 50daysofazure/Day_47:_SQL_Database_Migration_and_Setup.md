# Instruction

The Nautilus DevOps team needs to create an Azure SQL Database, export it to Azure Blob Storage, and download the backup to the `azure-client` host.

### Requirements

#### Azure SQL Database

| Setting           | Value                     |
| ----------------- | ------------------------- |
| Database          | `datacenter-sqldb`        |
| SQL Server        | `datacenter-server-26439` |
| Region            | `West US`                 |
| Compute           | Basic                     |
| Backup Redundancy | Locally-redundant         |
| Admin User        | `datacenter-admin`        |
| Database Size     | 2 GiB                     |

#### Storage

| Setting         | Value                        |
| --------------- | ---------------------------- |
| Storage Account | `datacenterst6696`           |
| Blob Container  | `datacenter-container-12841` |

#### Backup

| Setting     | Value                         |
| ----------- | ----------------------------- |
| Backup File | `datacenter-db-backup.bacpac` |

> **Note:** Use the Azure Portal (UI) to create and export the database. Use Azure CLI only to download the backup to the `azure-client` host.

# Solution

# Part 1: Create Azure SQL Database (UI)

## Step 1: Create SQL Database

Navigate to:

```text
SQL databases
```

Click:

```text
+ Create
```

## Basics

Configure:

| Setting        | Value                       |
| -------------- | --------------------------- |
| Database Name  | `datacenter-sqldb`          |
| Resource Group | Existing Lab Resource Group |

### Create New SQL Server

Click:

```text
Create New
```

Configure:

| Setting        | Value                     |
| -------------- | ------------------------- |
| Server Name    | `datacenter-server-26439` |
| Region         | `West US`                 |
| Authentication | SQL Authentication        |
| Admin Login    | `datacenter-admin`        |
| Password       | Choose a strong password  |

Click:

```text
OK
```

<img width="897" height="852" alt="create sql db server" src="https://github.com/user-attachments/assets/6d28d787-2da1-4ed3-90e3-abfc6e8ce82a" />

## Compute + Storage

Click:

```text
Configure Database
```

Choose:

```text
Basic
```

Database size:

```text
2 GiB
```

Save.

## Backup Storage Redundancy

Select:

```text
Locally-redundant backup storage (LRS)
```

Leave the remaining settings as default.

Click:

```text
Review + Create
```

Then:

```text
Create
```

<img width="652" height="842" alt="create sql db " src="https://github.com/user-attachments/assets/5b6d4e30-d005-4d3b-bd5b-596bce9304ab" />

Wait until the database status becomes:

```text
Ready
```

<img width="1755" height="447" alt="sql db" src="https://github.com/user-attachments/assets/f5e1ebc0-2bf4-4ebd-8d25-0287c37a8e67" />


# Part 2: Create Storage Account (UI)

Navigate:

```text
Storage Accounts
```

Click:

```text
+ Create
```

Configure:

| Setting     | Value              |
| ----------- | ------------------ |
| Name        | `datacenterst6696` |
| Region      | `West US`          |
| Performance | Standard           |
| Redundancy  | LRS                |

Create the storage account.


<img width="727" height="822" alt="create str acc" src="https://github.com/user-attachments/assets/f904511b-ed36-4899-b9fb-7d10c6884595" />

# Part 3: Create Blob Container (UI)

Open:

```text
datacenterst6696
```

Navigate:

```text
Containers
```

Click:

```text
+ Container
```

Configure:

| Setting       | Value                        |
| ------------- | ---------------------------- |
| Name          | `datacenter-container-12841` |
| Public Access | Private                      |

Click:

```text
Create
```

<img width="466" height="392" alt="container" src="https://github.com/user-attachments/assets/e8e2a7b3-0d65-4201-886f-b8d638574da0" />

# Part 4: Export SQL Database (UI)

Open:

```text
datacenter-sqldb
```

Click:

```text
Export
```

Configure:

### Storage

| Setting         | Value                         |
| --------------- | ----------------------------- |
| Storage Account | `datacenterst6696`            |
| Container       | `datacenter-container-12841`  |
| File Name       | `datacenter-db-backup.bacpac` |

### Authentication

| Setting  | Value                                            |
| -------- | ------------------------------------------------ |
| Username | `datacenter-admin`                               |
| Password | Same password used while creating the SQL Server |

Click:

```text
OK
```

<img width="661" height="672" alt="export db" src="https://github.com/user-attachments/assets/d07d2e82-bb0b-4285-866f-99dd2333ebe4" />

Export may take several minutes.

Wait until the export finishes successfully.

# Part 5: Verify Backup

Navigate:

```text
Storage Account
→ Containers
→ datacenter-container-12841
```

Verify the file exists:

```text
datacenter-db-backup.bacpac
```

<img width="955" height="517" alt="export verify" src="https://github.com/user-attachments/assets/cbbde1e5-bdd0-4935-bc33-9fb9edc46184" />

# Part 6: Download Backup to Azure Client

On the `azure-client` host:

Get the storage account key:

```bash
RG_NAME=$(az group list --query "[0].name" -o tsv)

ACCOUNT_KEY=$(az storage account keys list \
  --resource-group $RG_NAME \
  --account-name datacenterst6696 \
  --query "[0].value" \
  -o tsv)
```

Download the backup:

```bash
az storage blob download \
  --account-name datacenterst6696 \
  --account-key $ACCOUNT_KEY \
  --container-name datacenter-container-12841 \
  --name datacenter-db-backup.bacpac \
  --file /opt/datacenter-db-backup.bacpac
```

<img width="871" height="272" alt="downlaod" src="https://github.com/user-attachments/assets/f600eac3-6ba7-481d-bbf5-2a4102e69cd5" />

# Part 7: Verify Download

```bash
ls -lh /opt
```

Expected:

```text
datacenter-db-backup.bacpac
```

Verify:

```bash
file /opt/datacenter-db-backup.bacpac
```

<img width="690" height="80" alt="download verify" src="https://github.com/user-attachments/assets/2c59725e-f299-4c6a-b70e-6e926a97c536" />

The file should exist and be readable.


## Fun Message

*"Your Azure SQL database has been successfully exported, stored securely in Blob Storage, and downloaded for safekeeping—bringing your cloud backup workflow full circle 🚀🗄️☁️"*
