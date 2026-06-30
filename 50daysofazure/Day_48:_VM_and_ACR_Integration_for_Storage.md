# Instruction

The Nautilus DevOps team needs to deploy a containerized Python application on an Azure Virtual Machine using Azure Container Registry (ACR) and Azure Blob Storage.

### Requirements

#### Virtual Machine

| Setting          | Value             |
| ---------------- | ----------------- |
| VM Name          | `xfusion-vm`      |
| Region           | `East US`         |
| Authentication   | SSH Public Key    |
| Software         | Docker, Azure CLI |
| Application Port | 80                |

#### Azure Container Registry

| Setting    | Value                |
| ---------- | -------------------- |
| ACR Name   | `xfusionacr19536`    |
| Region     | `East US`            |
| Repository | `xfusion/python-app` |
| Tag        | `latest`             |

#### Storage

| Setting         | Value              |
| --------------- | ------------------ |
| Storage Account | `xfusionstor19536` |
| Container       | `xfusion-config`   |
| File            | `config.json`      |

> **Note:** Use the Azure Portal (UI) to create Azure resources. Use Azure CLI for building, pushing, and deploying the Docker image.

# Solution

# Part 1: Create Azure Container Registry (UI)

Navigate to:

```text
Container Registries
```

Click:

```text
+ Create
```

Configure:

| Setting       | Value             |
| ------------- | ----------------- |
| Registry Name | `xfusionacr19536` |
| Region        | `East US`         |
| SKU           | Basic             |

Click:

```text
Review + Create
```

Then:

```text
Create
```
Wait for deployment to finish.

<img width="595" height="797" alt="create container registerry" src="https://github.com/user-attachments/assets/61e1261f-d656-47f0-bbb6-da3f6e62cf96" />

# Part 2: Build and Push Docker Image

## Login to Azure

On the **azure-client** host:

```bash
az login
```

<img width="795" height="412" alt="az login" src="https://github.com/user-attachments/assets/88c54c52-9b2f-4daf-b20d-6ffe4549539a" />

Login using the provided credentials.

## Login to ACR

```bash
az acr login --name xfusionacr19536
```

<img width="656" height="117" alt="acr login" src="https://github.com/user-attachments/assets/efcab9b1-03e6-463e-bcd0-51d0ac80209f" />

## Build the Image

Navigate to the application directory:

```bash
cd /root/pyapp
```

Build the image:

```bash
docker build -t xfusion/python-app:latest .
```

<img width="997" height="271" alt="docker built" src="https://github.com/user-attachments/assets/249d294d-8df8-4960-8d51-5ca6a1201076" />

## Tag the Image

```bash
docker tag xfusion/python-app:latest \
xfusionacr19536.azurecr.io/xfusion/python-app:latest
```

## Push the Image

```bash
docker push \
xfusionacr19536.azurecr.io/xfusion/python-app:latest
```

<img width="932" height="252" alt="docker push" src="https://github.com/user-attachments/assets/0738d145-a02f-4987-a097-6ef46d276b70" />

Verify:

```bash
az acr repository list \
--name xfusionacr19536 \
-o table
```

Expected:

```text
xfusion/python-app
```

<img width="587" height="340" alt="acr verify" src="https://github.com/user-attachments/assets/fa34ce8c-38c4-4529-bc32-34a3a2f5ec5c" />

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

| Setting     | Value              |
| ----------- | ------------------ |
| Name        | `xfusionstor19536` |
| Region      | `East US`          |
| Performance | Standard           |
| Redundancy  | LRS                |

Create the storage account.

<img width="1092" height="806" alt="create storage acc" src="https://github.com/user-attachments/assets/86ce815d-3e8f-41f4-94dc-1cf5dee302f3" />

# Part 4: Create Blob Container (UI)

Open:

```text
xfusionstor19536
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

| Setting       | Value            |
| ------------- | ---------------- |
| Name          | `xfusion-config` |
| Public Access | Private          |

Click:

```text
Create
```

<img width="527" height="446" alt="create blobl" src="https://github.com/user-attachments/assets/ea6b475f-82c7-41a9-8371-30d86427aa67" />

# Part 5: Upload config.json

From the Azure Portal:

Open:

```text
xfusion-config
```

Click:

```text
Upload
```

Select:

```text
/root/config.json
```

Click:

```text
Upload
```

Verify that:

```text
config.json
```

<img width="1257" height="470" alt="config upload " src="https://github.com/user-attachments/assets/138c22b5-a840-4a2b-bdf9-5ce37a9cc6da" />

appears in the container.

# Part 6: Create Virtual Machine (UI)

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
| Name           | `xfusion-vm`   |
| Region         | `East US`      |
| Image          | Ubuntu Server  |
| Size           | Standard_B1s   |
| Authentication | SSH Public Key |

Select:

```text
Use existing public key
```

Paste the contents of:

<img width="1002" height="657" alt="key gen" src="https://github.com/user-attachments/assets/e177e5a7-5a62-4b84-a872-ad5f55ba8044" />

```bash
cat ~/.ssh/id_rsa.pub
```

Enable:

```text
Allow SSH (22)
```

<img width="1517" height="222" alt="ports" src="https://github.com/user-attachments/assets/26b93611-a417-4d82-9993-023c79353316" />

Create the VM.

<img width="851" height="831" alt="create vm" src="https://github.com/user-attachments/assets/59b1c112-81f3-4eea-9fc4-b7d7ca89cf8a" />

<img width="1792" height="802" alt="vm" src="https://github.com/user-attachments/assets/f6965279-cef6-4ba7-808f-bbb3610a6483" />

# Part 7: Install Docker and Azure CLI

SSH into the VM:

```bash
ssh azureuser@<VM_PUBLIC_IP>
```

<img width="766" height="397" alt="ssh" src="https://github.com/user-attachments/assets/80561547-89e2-42ba-b193-d67f21bbc765" />

Install Docker:

```bash
sudo apt update
sudo apt install docker.io -y

sudo systemctl enable docker
sudo systemctl start docker

sudo usermod -aG docker azureuser
newgrp docker
```

<img width="787" height="172" alt="update and install" src="https://github.com/user-attachments/assets/92c0e537-000c-4002-a67e-fe2b14786761" />

Install Azure CLI:

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

<img width="752" height="102" alt="install azure" src="https://github.com/user-attachments/assets/f334eaf3-1fad-40f7-8213-3310960d9d9c" />

Verify:

```bash
docker --version
az version
```


# Part 8: Pull and Run the Docker Image

Login to Azure:

```bash
az login
```

<img width="795" height="412" alt="az login" src="https://github.com/user-attachments/assets/381b821a-ab65-4cf1-8ce7-c6088d45507f" />

Login to ACR:

```bash
az acr login --name xfusionacr19536
```

<img width="656" height="117" alt="acr login" src="https://github.com/user-attachments/assets/ad62d218-e07f-42ab-86db-523f2485dc3d" />

Pull the image:

```bash
docker pull \
xfusionacr19536.azurecr.io/xfusion/python-app:latest
```

<img width="926" height="302" alt="docker pull" src="https://github.com/user-attachments/assets/02465b73-d0f5-4503-b245-0c079c89eefb" />

Run the container:

```bash
docker run -d \
--name python-app \
-p 80:80 \
xfusionacr19536.azurecr.io/xfusion/python-app:latest
```

<img width="1002" height="391" alt="docker run" src="https://github.com/user-attachments/assets/839da983-0396-4e8e-b270-d7c0c48f378e" />

Verify:

```bash
docker ps
```

Expected:

```text
Up
```

# Part 9: Verify the Application

Open:

```text
http://<VM_PUBLIC_IP>
```

or

```bash
curl http://<VM_PUBLIC_IP>
```

<img width="582" height="222" alt="verify" src="https://github.com/user-attachments/assets/6cbb1af2-1a03-4a72-8b75-b23966f309bd" />

The Python application should respond successfully.


## Fun Message

*"Your containerized Python application is now running on Azure, powered by Azure Container Registry for image delivery and Azure Blob Storage for configuration management 🚀🐳☁️"*
