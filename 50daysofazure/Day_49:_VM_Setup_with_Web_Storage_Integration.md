# Instruction

The Nautilus DevOps team needs to deploy a Virtual Machine that serves a static web page downloaded securely from Azure Blob Storage.

### Requirements

#### Virtual Network

| Setting | Value             |
| ------- | ----------------- |
| VNet    | `nautilus-vnet`   |
| Subnet  | `nautilus-subnet` |
| Region  | `East US`         |

#### Storage

| Setting         | Value                |
| --------------- | -------------------- |
| Storage Account | `nautilusstor25754`  |
| Region          | `East US`            |
| Redundancy      | LRS                  |
| Container       | `nautilus-container` |
| Blob            | `index.html`         |
| Public Access   | Disabled             |

#### Virtual Machine

| Setting        | Value          |
| -------------- | -------------- |
| VM Name        | `nautilus-vm`  |
| Region         | `East US`      |
| Authentication | SSH Public Key |

> **Note:** Use the Azure Portal (UI) to create the Azure resources. Use Azure CLI on the VM only to download the blob.

# Solution

# Part 1: Create Virtual Network (UI)

Navigate to:

```text
Virtual Networks
```

Click:

```text
+ Create
```

Configure:

| Setting | Value           |
| ------- | --------------- |
| Name    | `nautilus-vnet` |
| Region  | `East US`       |

Under **IP Addresses**, create a subnet:

| Setting       | Value                         |
| ------------- | ----------------------------- |
| Name          | `nautilus-subnet`             |
| Address Range | Default (or any valid subnet) |

Click:

```text
Review + Create
```

<img width="691" height="775" alt="create vnet" src="https://github.com/user-attachments/assets/a118da4b-567d-4803-90b3-c5dd6596538f" />

Then:

```text
Create
```

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

| Setting     | Value                   |
| ----------- | ----------------------- |
| Name        | `nautilusstor25754`     |
| Region      | `East US`               |
| Performance | Standard                |
| Redundancy  | Locally-redundant (LRS) |

Click **Next: Advanced** (or **Networking**, depending on the portal version) and ensure:

```text
Allow Blob Anonymous Access = Disabled
```

Complete:

```text
Review + Create
```

<img width="542" height="786" alt="create storage acc" src="https://github.com/user-attachments/assets/5047b4c7-324a-4384-bbcb-c5437fd421c3" />

Create the storage account.

# Part 3: Create Blob Container (UI)

Open:

```text
nautilusstor25754
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

| Setting             | Value                         |
| ------------------- | ----------------------------- |
| Name                | `nautilus-container`          |
| Public Access Level | Private (No anonymous access) |

Click:

```text
Create
```

<img width="472" height="401" alt="create container" src="https://github.com/user-attachments/assets/3ee0512a-25e7-4983-8ef7-3a18b3190613" />

# Part 4: Upload index.html (UI)

Open:

```text
nautilus-container
```

Click:

```text
Upload
```

Select:

```text
/root/index.html
```

Click:

```text
Upload
```

Verify:

```text
index.html
```

<img width="1317" height="421" alt="index file upload" src="https://github.com/user-attachments/assets/243dc2eb-7eec-4f97-9efd-40ed13f03073" />

appears in the container.

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

| Setting | Value         |
| ------- | ------------- |
| Name    | `nautilus-vm` |
| Region  | `East US`     |
| Image   | Ubuntu Server |
| Size    | Standard_B1s  |

Authentication:

Select:

```text
SSH Public Key
```

Choose:

```text
Use existing public key
```

Paste:

```bash
cat ~/.ssh/id_rsa.pub
```

<img width="1020" height="552" alt="keygen" src="https://github.com/user-attachments/assets/9b28bc99-f52f-4f3f-9260-fdb77cd3e1c5" />

from the `azure-client` host.

## Networking

Select:

| Setting         | Value                  |
| --------------- | ---------------------- |
| Virtual Network | `nautilus-vnet`        |
| Subnet          | `nautilus-subnet`      |
| Public IP       | Enabled                |
| Inbound Ports   | SSH (22) and HTTP (80) |

<img width="747" height="767" alt="create vm" src="https://github.com/user-attachments/assets/172fd54e-1e24-430a-ac64-d5b9df801202" />

Create the VM.

<img width="1762" height="536" alt="vm" src="https://github.com/user-attachments/assets/5072bbd4-0777-4a08-9ea1-202c5deafd68" />

# Part 6: Configure the VM

SSH into the VM:

```bash
ssh azureuser@<VM_PUBLIC_IP>
```

<img width="687" height="231" alt="ssh" src="https://github.com/user-attachments/assets/513a2acb-4120-473d-b829-abc4a72d106c" />

## Install Nginx

```bash
sudo apt update
sudo apt install nginx -y

sudo systemctl enable nginx
sudo systemctl start nginx
```

## Install Azure CLI

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

<img width="995" height="147" alt="add microsoft key" src="https://github.com/user-attachments/assets/c4d5d19b-26da-4432-9922-4401602e503c" />

Verify:

```bash
az version
```

<img width="365" height="147" alt="az version" src="https://github.com/user-attachments/assets/b3cc9b5f-5224-4af7-a10e-03b2d6ac2547" />

## Login to Azure

```bash
az login
```

Use the lab credentials.

## Get the Storage Account Key

From the `azure-client` host (or Azure Cloud Shell):

```bash
az storage account keys list \
  --resource-group <RESOURCE_GROUP> \
  --account-name nautilusstor25754 \
  -o table
```

Copy one of the keys.

## Download index.html

Run on the VM:

```bash
sudo az storage blob download \
  --account-name nautilusstor25754 \
  --account-key <ACCOUNT_KEY> \
  --container-name nautilus-container \
  --name index.html \
  --file /var/www/html/index.html
```

<img width="890" height="366" alt="download from blob to vm" src="https://github.com/user-attachments/assets/ba64d407-2e47-4e39-94f1-bd442cb23989" />

<img width="575" height="232" alt="download from blob to vm verify" src="https://github.com/user-attachments/assets/2749957e-f4a6-4b47-800a-0b98f59e946a" />

Replace:

```text
<ACCOUNT_KEY>
```

with the copied storage account key.

## Restart Nginx

```bash
sudo systemctl restart nginx
```

Verify:

```bash
sudo systemctl status nginx
```

Expected:

```text
active (running)
```

<img width="1021" height="365" alt="website setup" src="https://github.com/user-attachments/assets/ff6f25ac-fb0d-4705-9f0d-c1ae1e803294" />

# Part 7: Verify the Website

Open a browser:

```text
http://<VM_PUBLIC_IP>
```

<img width="700" height="322" alt="verify ui" src="https://github.com/user-attachments/assets/97bbcfbf-0dfd-4d3e-97c8-4283eb2f2938" />

or use:

```bash
curl http:localhost
```

<img width="652" height="242" alt="verify cli" src="https://github.com/user-attachments/assets/612cb9d5-bea4-4650-a460-e16c7a35234e" />

The contents of:

```text
/root/index.html
```

should be displayed.

## Fun Message

*"Your VM now securely retrieves static content from private Azure Blob Storage and serves it through Nginx—keeping the storage private while making the web content publicly accessible 🚀☁️🌐"*
