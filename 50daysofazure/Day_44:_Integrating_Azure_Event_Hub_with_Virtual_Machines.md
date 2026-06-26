# Instruction

The Nautilus DevOps team needs to integrate an Azure Virtual Machine with **Azure Event Hubs** for centralized log collection.

### Requirements

#### Event Hubs Namespace

| Setting      | Value                |
| ------------ | -------------------- |
| Namespace    | `nautilus-namespace` |
| Region       | `East US`            |
| Pricing Tier | `Standard`           |
| Auto Inflate | Enabled              |

#### Event Hub

| Setting        | Value          |
| -------------- | -------------- |
| Event Hub Name | `nautilus-hub` |

#### Virtual Machine

| Setting | Value                          |
| ------- | ------------------------------ |
| VM      | `nautilus-vm`                  |
| Script  | `/home/azureuser/send_logs.py` |

### Validation

* Execute `send_logs.py` multiple times.
* Verify Event Hub metrics show incoming events.

> **Note:** Use the Azure Portal (UI) to create Event Hubs resources. Use SSH only to execute the Python script on the VM.

# Solution

## Part 1: Create Event Hubs Namespace

### Step 1: Login to Azure Portal

Open:

```text
https://portal.azure.com
```

### Step 2: Create Event Hubs Namespace

Search:

```text
Event Hubs
```

Click:

```text
+ Create
```

Configure:

| Setting        | Value                       |
| -------------- | --------------------------- |
| Resource Group | Existing Lab Resource Group |
| Namespace      | `nautilus-namespace`        |
| Region         | `East US`                   |
| Pricing Tier   | `Standard`                  |

Click:

```text
Next : Features
```

### Step 3: Enable Auto Inflate

Under **Features**:

Enable:

```text
Auto-inflate
```

Leave the maximum throughput units at the default value unless instructed otherwise.

Continue with the remaining defaults.

Click:

```text
Review + Create
```

<img width="641" height="842" alt="create ns" src="https://github.com/user-attachments/assets/fc2dca9f-5ffb-4a22-b495-063a2e20c5f3" />

Then:

```text
Create
```

Wait until deployment completes.

# Part 2: Create the Event Hub

Open:

```text
nautilus-namespace
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

Configure:

| Setting | Value          |
| ------- | -------------- |
| Name    | `nautilus-hub` |

Leave all other settings at their defaults.

Click:

```text
Create
```

<img width="525" height="842" alt="create hb" src="https://github.com/user-attachments/assets/8f7d4e21-a846-451c-aa1e-5df1aaedb97d" />

# Part 3: Get the VM Public IP

Open:

```text
Virtual Machines
```

Select:

```text
nautilus-vm
```

Copy the:

```text
Public IP Address
```

<img width="1546" height="337" alt="vm" src="https://github.com/user-attachments/assets/5e0b801e-f7f4-4a99-83e4-0a830c5551f6" />

# Part 4: SSH into the VM

From the `azure-client` host:

```bash
ssh azureuser@<VM_PUBLIC_IP>
```

<img width="792" height="247" alt="ssh" src="https://github.com/user-attachments/assets/62ef64f7-cdc7-4f86-b660-522810b44f25" />

# Part 5: Verify the Script Exists

```bash
ls -l /home/azureuser/send_logs.py
```

Expected:

```text
/home/azureuser/send_logs.py
```

<img width="947" height="90" alt="send_logs file" src="https://github.com/user-attachments/assets/1d9eedbf-ec4b-41af-a7b1-8a1bdf4f776b" />

# Part 6: Retrieve the Event Hub Connection String

Open:

```text
nautilus-namespace
```

Navigate to:

```text
Shared access policies
```

Open:

```text
RootManageSharedAccessKey
```

Copy the:

```text
Primary Connection String
```

It will look similar to:

```text
Endpoint=sb://nautilus-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=xxxxxxxxxxxxxxxx
```

<img width="1877" height="595" alt="shared access policies" src="https://github.com/user-attachments/assets/00333ad8-aa34-469b-8ef3-48cc650f51a9" />

# Part 7: Update `send_logs.py`

Open the script:

```bash
vi /home/azureuser/send_logs.py
```

Locate the placeholder, for example:

```python
CONNECTION_STR = "<EVENT_HUB_CONNECTION_STRING>"
```

or

```python
connection_str = "YOUR_CONNECTION_STRING"
```

Replace it with the connection string copied from Azure Portal:

```python
CONNECTION_STR = "Endpoint=sb://nautilus-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=xxxxxxxxxxxxxxxx"
```

Save the file.

# Part 8: Verify the Event Hub Name

If the script also contains an Event Hub placeholder such as:

```python
EVENT_HUB_NAME = "<EVENT_HUB_NAME>"
```

replace it with:

```python
EVENT_HUB_NAME = "nautilus-hub"
```

# Part 9: Execute the Script Multiple Times

Run:

```bash
python3 /home/azureuser/send_logs.py
```

<img width="905" height="300" alt="send logs" src="https://github.com/user-attachments/assets/b9fc2a37-be1c-4a4f-8485-920c18e40d57" />

Execute it several times, for example:

```bash
for i in {1..5}; do
    python3 /home/azureuser/send_logs.py
done
```

If the lab expects more events, you can run it 10 times:

```bash
for i in {1..10}; do
    python3 /home/azureuser/send_logs.py
done
```

<img width="947" height="221" alt="send_logs loop" src="https://github.com/user-attachments/assets/c9c7606e-1fc0-4be1-9636-c23bae09a41f" />

# Part 10: Verify Event Hub Metrics

Return to the Azure Portal.

Open:

```text
Event Hubs
```

Select:

```text
nautilus-namespace
```

Open:

```text
nautilus-hub
```

Navigate to:

```text
Monitoring
→ Metrics
```

Configure:

| Setting | Value             |
| ------- | ----------------- |
| Metric  | Incoming Messages |

or

```text
Incoming Requests
```

Wait 1–2 minutes for Azure metrics to update.

You should observe values greater than zero.

<img width="1892" height="816" alt="metrics" src="https://github.com/user-attachments/assets/e62df699-f4b5-42f1-8506-e81e59291d03" />

## Fun Message

*"Your Azure Event Hub is now collecting logs from the VM, giving you a scalable foundation for centralized event streaming and monitoring 🚀📡☁️"*
