# Instruction

The Nautilus DevOps team needs to integrate an Azure Virtual Machine with **Azure Event Hubs** and **Azure Blob Storage** for centralized log collection and backup.

### Requirements

#### Event Hubs

| Setting      | Value              |
| ------------ | ------------------ |
| Namespace    | `devops-namespace` |
| Event Hub    | `devops-hub`       |
| Region       | `East US`          |
| Tier         | Standard           |
| Auto Inflate | Enabled            |

#### Storage Account

| Setting         | Value                                       |
| --------------- | ------------------------------------------- |
| Storage Account | `devopsst14413`                             |
| Container       | `devops-backup-11382`                       |
| Access Level    | Blob (anonymous read access for blobs only) |

#### Virtual Machine

| Setting | Value       |
| ------- | ----------- |
| VM Name | `devops-vm` |
| Region  | `East US`   |

> **Note:** Use the Azure Portal (UI) to create Azure resources. Use SSH to copy and modify the Python script.

# Solution

## Part 1: Create Event Hubs Namespace (UI)

Navigate to:

```text
Event Hubs
```

Click:

```text
+ Create
```

Configure:

| Setting      | Value              |
| ------------ | ------------------ |
| Namespace    | `devops-namespace` |
| Region       | `East US`          |
| Pricing Tier | Standard           |

Under **Features**:

Enable:

```text
Auto Inflate
```

Click:

```text
Review + Create
```

<img width="777" height="842" alt="review ns" src="https://github.com/user-attachments/assets/b35127d9-1066-4ea7-bebf-9edfd5632d87" />

Then:

```text
Create
```

# Part 2: Create Event Hub (UI)

Open:

```text
devops-namespace
```

Navigate:

```text
Entities
→ Event Hubs
```

Click:

```text
+ Event Hub
```

Name:

```text
devops-hub
```

Click:

```text
Create
```

<img width="957" height="652" alt="create event hub" src="https://github.com/user-attachments/assets/40a398be-1204-4510-9081-051d44281711" />

# Part 3: Create Storage Account (UI)

Navigate:

```text
Storage Accounts
```

Click:

```text
+ Create
```

Configure:

| Setting     | Value           |
| ----------- | --------------- |
| Name        | `devopsst14413` |
| Region      | `East US`       |
| Performance | Standard        |
| Redundancy  | LRS             |

Click:

```text
Review + Create
```

<img width="1092" height="532" alt="create stacc" src="https://github.com/user-attachments/assets/1e11f11d-a741-43aa-9b8e-804e4e91437d" />

Create the storage account.

# Part 4: Create Blob Container (UI)

Open:

```text
devopsst14413
```

Navigate:

```text
Data Storage
→ Containers
```

Click:

```text
+ Container
```

Configure:

| Setting             | Value                                       |
| ------------------- | ------------------------------------------- |
| Name                | `devops-backup-11382`                       |
| Public Access Level | Blob (anonymous read access for blobs only) |

Click:

```text
Create
```

<img width="512" height="495" alt="create blob container" src="https://github.com/user-attachments/assets/56b0e840-93eb-40ff-83bd-899f21b80f64" />

# Part 5: Create the Virtual Machine (UI)

Navigate:

```text
Virtual Machines
```

Click:

```text
+ Create
```

Configure:

| Setting        | Value          |
| -------------- | -------------- |
| Name           | `devops-vm`    |
| Region         | `East US`      |
| Image          | Ubuntu Server  |
| Size           | Standard_B1s   |
| Authentication | SSH Public Key |

Create the VM and wait until deployment completes.

<img width="825" height="792" alt="create vm" src="https://github.com/user-attachments/assets/74d3a9c9-d1f5-42ac-aaf8-9fbebd22ac6c" />

# Part 6: Copy the Script to the VM

Get the VM public IP from the Azure Portal.

From the **client host**:

```bash
scp /root/send_logs.py azureuser@<VM_PUBLIC_IP>:/home/azureuser/
```

<img width="952" height="77" alt="scp run" src="https://github.com/user-attachments/assets/1d49ccc1-4132-4b6b-acde-38fa5225226e" />

SSH into the VM:

```bash
ssh azureuser@<VM_PUBLIC_IP>
```

Verify:

```bash
ls -l /home/azureuser/send_logs.py
```

<img width="307" height="102" alt="scp verify" src="https://github.com/user-attachments/assets/aff80fdc-b331-4af4-b585-cc9047ddd48c" />

# Part 7: Configure Event Hub Connection

From the Azure Portal:

Open:

```text
devops-namespace
→ Shared access policies
→ RootManageSharedAccessKey
```

Copy the **Primary Connection String**.

Edit the script:

```bash
vi /home/azureuser/send_logs.py
```

Replace the placeholder:

```python
CONNECTION_STR = "<EVENT_HUB_CONNECTION_STRING>"
```

with the copied connection string.

Replace:

```python
EVENT_HUB_NAME = "<EVENT_HUB_NAME>"
```

with:

```python
EVENT_HUB_NAME = "devops-hub"
```

# Part 8: Configure Blob Storage Backup

Get the storage account access key from the Azure Portal:

```text
Storage Account
→ Access Keys
```

Copy **key1**.

Modify `send_logs.py` to include Blob Storage configuration.

Add:

```python
from azure.storage.blob import BlobServiceClient
import uuid

STORAGE_CONNECTION_STRING = "<STORAGE_CONNECTION_STRING>"
CONTAINER_NAME = "devops-backup-11382"

blob_service = BlobServiceClient.from_connection_string(
    STORAGE_CONNECTION_STRING
)

container = blob_service.get_container_client(CONTAINER_NAME)
```

After sending each log to Event Hub, upload it to Blob Storage:

```python
blob_name = f"log-{uuid.uuid4()}.txt"

container.upload_blob(
    blob_name,
    log_message,
    overwrite=True
)
```

Replace:

```text
<STORAGE_CONNECTION_STRING>
```

with the Storage Account connection string from:

```text
Storage Account
→ Access Keys
→ Connection String
```

Save the file.

# Part 9: Install Required Python Packages

<img width="851" height="185" alt="update and install" src="https://github.com/user-attachments/assets/71d74bb7-2336-4cc5-bbd2-ffa320516926" />

If necessary:

```bash
pip3 install azure-eventhub azure-storage-blob
```

# Part 10: Execute the Script

Run it several times:

```bash
for i in {1..10}; do
    python3 /home/azureuser/send_logs.py
done
```

<img width="682" height="72" alt="log send from vm to blob" src="https://github.com/user-attachments/assets/af82749f-efd3-469e-aaec-a3ed08b6b1c6" />

# Part 11: Verify Event Hub

Portal:

```text
Event Hubs
→ devops-namespace
→ devops-hub
→ Monitoring
→ Metrics
```

Metric:

```text
Incoming Messages
```

<img width="1880" height="780" alt="metrics" src="https://github.com/user-attachments/assets/44605949-86dd-4deb-bada-32b74ff3e985" />

After a minute, values should increase.

# Part 12: Verify Blob Storage

Portal:

```text
Storage Account
→ Containers
→ devops-backup-11382
```

You should see several uploaded log files, for example:

```text
log-xxxxxxxx.txt
```

<img width="1491" height="467" alt="log send blob" src="https://github.com/user-attachments/assets/c1f5a635-a9c6-4cb2-b058-a5acf7c4a146" />

Open one to confirm it contains the log message.

## Fun Message

*"Your VM now streams logs to Azure Event Hubs while simultaneously backing them up to Azure Blob Storage, providing both real-time ingestion and durable storage 🚀📡📦☁️"*
