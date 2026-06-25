# Instruction

The Nautilus Development Team needs to deploy an Azure Virtual Machine running Nginx behind an Azure Application Gateway for HTTP traffic management.

### Requirements

#### Network Security Group

| Setting   | Value        |
| --------- | ------------ |
| NSG Name  | `devops-nsg` |
| Rule Name | `Allow-HTTP` |
| Protocol  | TCP          |
| Port      | `80`         |

#### Virtual Machine

| Setting        | Value          |
| -------------- | -------------- |
| VM Name        | `devops-vm`    |
| Region         | `West US`      |
| Image          | Any Ubuntu     |
| Size           | `Standard_B1s` |
| Authentication | SSH Public Key |
| OS Disk        | Standard HDD   |
| NSG            | `devops-nsg`   |

#### Application Gateway

| Setting         | Value                  |
| --------------- | ---------------------- |
| Name            | `devops-agw`           |
| Public IP       | `devops-agw-ip`        |
| Backend Pool    | `devops-backendpool`   |
| Backend Setting | `devops-http-settings` |
| Listener        | `devops-listener`      |
| Routing Rule    | `devops-routing-rule`  |

> **Note:** Use the Azure Portal (UI) to complete the task.

# Solution

## Part 1: Create the Network Security Group

### 1. Create NSG

Navigate to:

```text
Network Security Groups
```

Click:

```text
+ Create
```

Configure:

| Setting        | Value                       |
| -------------- | --------------------------- |
| Name           | `devops-nsg`                |
| Region         | `West US`                   |
| Resource Group | Existing Lab Resource Group |

Click:

```text
Review + Create
```

Then:

```text
Create
```

### 2. Add HTTP Rule

Open:

```text
devops-nsg
```

Navigate:

```text
Inbound security rules
```

Click:

```text
+ Add
```

Configure:

| Setting          | Value        |
| ---------------- | ------------ |
| Source           | Any          |
| Protocol         | TCP          |
| Destination Port | 80           |
| Action           | Allow        |
| Priority         | 100          |
| Name             | `Allow-HTTP` |

Click:

```text
Add
```

<img width="1887" height="502" alt="network nsg inbound rule" src="https://github.com/user-attachments/assets/db69ffea-aa8a-41fc-940b-b45216bae4a9" />

# Part 2: Create the Virtual Machine

Navigate:

```text
Virtual Machines
```

Click:

```text
+ Create
```

## Basics

| Setting | Value          |
| ------- | -------------- |
| Name    | `devops-vm`    |
| Region  | `West US`      |
| Image   | Ubuntu Server  |
| Size    | `Standard_B1s` |

Authentication:

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

from the `azure-client` host.

## Disks

Select:

```text
Standard HDD
```

## Networking

Choose:

```text
Existing NSG
```

Select:

```text
devops-nsg
```

Leave Public IP enabled.

<img width="637" height="562" alt="network nsg" src="https://github.com/user-attachments/assets/984002ec-ef4e-4e52-9dc7-1d7d2f46d6ce" />

## Advanced

Locate:

```text
Custom Data
```

Paste:

```yaml
#cloud-config
package_update: true
packages:
  - nginx

runcmd:
  - systemctl enable nginx
  - systemctl start nginx
```

Create the VM.

Wait until deployment finishes.

# Part 3: Create the Application Gateway

Navigate:

```text
Application Gateways
```

Click:

```text
+ Create
```

## Basics

| Setting | Value        |
| ------- | ------------ |
| Name    | `devops-agw` |
| Region  | `West US`    |
| Tier    | Standard V2  |

## Frontends

Create a Public IP:

| Setting | Value           |
| ------- | --------------- |
| Name    | `devops-agw-ip` |

## Networking

Choose the same VNet as the VM.

Create a dedicated subnet:

| Setting | Value              |
| ------- | ------------------ |
| Name    | `appGatewaySubnet` |

Example CIDR:

```text
10.0.1.0/24
```

> **Important:** The Application Gateway must be deployed in its own subnet, separate from the VM subnet.

## Backends

Click:

```text
Add Backend Pool
```

Configure:

| Setting | Value                |
| ------- | -------------------- |
| Name    | `devops-backendpool` |

Click:

```text
Add Backend Target
```

Target Type:

```text
Virtual Machine
```

Select:

```text
devops-vm
```

Save.

## Backend Settings

Click:

```text
Add Backend Setting
```

Configure:

| Setting  | Value                  |
| -------- | ---------------------- |
| Name     | `devops-http-settings` |
| Protocol | HTTP                   |
| Port     | 80                     |

Save.

## Listener

Click:

```text
Add Listener
```

Configure:

| Setting  | Value             |
| -------- | ----------------- |
| Name     | `devops-listener` |
| Frontend | Public            |
| Protocol | HTTP              |
| Port     | 80                |

Save.

## Routing Rule

Click:

```text
Add Routing Rule
```

Configure:

| Setting         | Value                  |
| --------------- | ---------------------- |
| Name            | `devops-routing-rule`  |
| Listener        | `devops-listener`      |
| Backend Pool    | `devops-backendpool`   |
| Backend Setting | `devops-http-settings` |

Save.

Click:

```text
Review + Create
```

Then:

```text
Create
```

Deployment typically takes **5–10 minutes**.

Wait until the Application Gateway status is:

```text
Succeeded
```

<img width="1642" height="437" alt="agw" src="https://github.com/user-attachments/assets/7654d770-f384-4638-b877-4c3efebf712a" />

# Part 4: Verify Nginx

SSH into the VM:

```bash
ssh azureuser@<VM_PUBLIC_IP>
```

Verify:

```bash
sudo systemctl status nginx
```

Expected:

```text
active (running)
```

If needed:

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl restart nginx
```

Test locally:

```bash
curl localhost
```

Expected:

```html
Welcome to nginx!
```

# Part 5: Verify Application Gateway

Open:

```text
devops-agw
```

Copy the Public IP:

```text
devops-agw-ip
```

Open in a browser:

```text
http://<APPLICATION_GATEWAY_PUBLIC_IP>
```

or run:

```bash
curl http://<APPLICATION_GATEWAY_PUBLIC_IP>
```

Expected:

```html
Welcome to nginx!
```

<img width="1332" height="451" alt="verify" src="https://github.com/user-attachments/assets/16d75ce3-c473-4e87-8cbe-fe07bfe8c584" />

## Fun Message

*"Your Azure Application Gateway is now routing HTTP traffic to an Nginx-powered VM, providing a solid foundation for scalable web applications 🚀🌐☁️"*
