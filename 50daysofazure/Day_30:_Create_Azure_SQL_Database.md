# Instruction

The Nautilus DevOps team needs to create a **publicly accessible Azure SQL Database**.

### Requirements

| Setting                   | Value                              |
| ------------------------- | ---------------------------------- |
| Database Name             | `nautilus-sqldb`                   |
| SQL Server Name           | `nautilus-server-26349`            |
| Region                    | `Central US`                       |
| Compute Tier              | `Basic`                            |
| Database Size             | `2 GiB`                            |
| Backup Storage Redundancy | `Locally-redundant backup storage` |
| Admin Username            | `nautilus-admin`                   |
| Public Access             | Enabled                            |
| Final State               | `Ready`                            |

> **Note:** Use the Azure Portal (UI) to complete this task.



# Solution

## Step 1: Login to Azure Portal

Open:

```text
https://portal.azure.com
```

Login using the lab credentials.



# Step 2: Open SQL Databases

Search:

```text
SQL Databases
```

Click:

```text
+ Create
```



# Step 3: Configure Basic Information

### Basics Tab

| Setting        | Value                       |
| -------------- | --------------------------- |
| Resource Group | Existing lab resource group |
| Database Name  | `nautilus-sqldb`            |



## Create New SQL Server

Under:

```text
Server
```

Click:

```text
Create New
```

Configure:

| Setting               | Value                    |
| --------------------- | ------------------------ |
| Server Name           | `nautilus-server-26349`  |
| Location              | `Central US`             |
| Authentication Method | SQL Authentication       |
| Server Admin Login    | `nautilus-admin`         |
| Password              | Choose a strong password |
| Confirm Password      | Same password            |

Click:

```text
OK
```



# Step 4: Configure Compute + Storage

Under:

```text
Compute + Storage
```

Click:

```text
Configure Database
```

Select:

```text
Basic
```

(For less demanding workloads)

Verify:

| Setting       | Value |
| ------------- | ----- |
| Service Tier  | Basic |
| Max Data Size | 2 GB  |

If the size is configurable, select:

```text
2 GiB
```

Click:

```text
Apply
```

<img width="957" height="747" alt="dbsize" src="https://github.com/user-attachments/assets/397868ff-65ee-4c7a-b4e5-2ccbedbba32e" />

# Step 5: Configure Backup Redundancy

Locate:

```text
Backup Storage Redundancy
```

Select:

```text
Locally-redundant backup storage
```



# Step 6: Configure Networking

Navigate to:

```text
Networking
```

Under:

```text
Network Connectivity
```

Select:

```text
Public Endpoint
```

Ensure:

```text
Allow Azure services and resources to access this server
```

is enabled if available.

For lab purposes, public access must remain enabled.



# Step 7: Leave Remaining Settings as Default

Leave:

* Security
* Additional Settings
* Tags

at their default values.



# Step 8: Create the Database

Click:

```text
Review + Create
```

Wait for validation.

Click:

```text
Create
```

Deployment may take a few minutes.

<img width="722" height="540" alt="dbreview" src="https://github.com/user-attachments/assets/eb214347-ee5a-45a0-a8a1-17c93a113b15" />

# Step 9: Verify Deployment

After deployment completes:

Open:

```text
SQL Databases
```

Select:

```text
nautilus-sqldb
```

Verify:

| Property          | Expected Value          |
| ----------------- | ----------------------- |
| Name              | `nautilus-sqldb`        |
| Status            | `Online / Ready`        |
| Server            | `nautilus-server-26349` |
| Region            | `Central US`            |
| Tier              | `Basic`                 |
| Backup Redundancy | `Locally-redundant`     |

<img width="1897" height="576" alt="sqldb" src="https://github.com/user-attachments/assets/0d31230d-7ff2-4fa1-815c-ebd69d2a285d" />
