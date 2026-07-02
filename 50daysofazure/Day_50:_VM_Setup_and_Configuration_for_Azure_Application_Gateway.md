# Instruction

The Nautilus DevOps team needs to deploy two Nginx web servers behind an Azure Application Gateway for load balancing.

### Requirements

#### Virtual Network

| Setting | Value             |
| ------- | ----------------- |
| VNet    | `datacenter-vnet` |
| Region  | `East US`         |

#### Subnets

| Name                     | Purpose             |
| ------------------------ | ------------------- |
| `datacenter-subnet`      | Virtual Machines    |
| `datacenter-apgw-subnet` | Application Gateway |

#### Virtual Machines

| Setting | Value            |
| ------- | ---------------- |
| VM1     | `datacenter-vm1` |
| VM2     | `datacenter-vm2` |
| Image   | Ubuntu           |
| Size    | Standard_B1s     |

#### Application Gateway

| Setting     | Value                |
| ----------- | -------------------- |
| Name        | `datacenter-apgw`    |
| Frontend IP | `datacenter-apgw-ip` |

> **Note:** Use the Azure Portal (UI) for resource creation. SSH into the VMs only to install Nginx and customize the web pages.

# Solution

# Part 1: Create the Virtual Network (UI)

Navigate to:

```text
Virtual Networks
```

Click:

```text
+ Create
```

Configure:

| Setting | Value             |
| ------- | ----------------- |
| Name    | `datacenter-vnet` |
| Region  | `East US`         |

Under **IP Addresses**, create the following subnets:

| Subnet                   | Example CIDR  |
| ------------------------ | ------------- |
| `datacenter-subnet`      | `10.0.1.0/24` |
| `datacenter-apgw-subnet` | `10.0.2.0/24` |

> **Important:** The Application Gateway must be deployed in its own dedicated subnet.

<img width="672" height="782" alt="create vnet" src="https://github.com/user-attachments/assets/0f0d95d2-c94e-445d-bfc8-b7583ff5d06c" />

Click **Review + Create** → **Create**.

# Part 2: Create Virtual Machines (UI)

Create **two** Ubuntu VMs.

## VM1

| Setting        | Value               |
| -------------- | ------------------- |
| Name           | `datacenter-vm1`    |
| Region         | `East US`           |
| VNet           | `datacenter-vnet`   |
| Subnet         | `datacenter-subnet` |
| Authentication | SSH Public Key      |
| Size           | `Standard_B1s`      |

Allow inbound:

* SSH (22)
* HTTP (80)

<img width="1747" height="532" alt="vm1" src="https://github.com/user-attachments/assets/596eda4f-91b2-4374-865c-e5edc9b6e459" />

Create the VM.

## VM2

Repeat the same steps.

| Setting | Value            |
| ------- | ---------------- |
| Name    | `datacenter-vm2` |

<img width="1782" height="612" alt="vm2" src="https://github.com/user-attachments/assets/d5f482eb-99bc-4a1b-96a8-e2e75fad8db1" />

Everything else remains the same.

# Part 3: Install Nginx

SSH into **VM1**:

```bash
ssh azureuser@<VM1_PUBLIC_IP>
```

Install Nginx:

```bash
sudo apt update
sudo apt install nginx -y
```

Create the web page:

```bash
echo "Welcome to KKE Labs:Version 1" | sudo tee /var/www/html/index.html
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

Verify:

```bash
curl localhost
```

Expected:

```text
Welcome to KKE Labs:Version 1
```

<img width="577" height="221" alt="vm1 verify" src="https://github.com/user-attachments/assets/4fa83d16-3f6f-4e9a-bd9b-13cfca186424" />

SSH into **VM2**:

```bash
ssh azureuser@<VM2_PUBLIC_IP>
```

Install Nginx:

```bash
sudo apt update
sudo apt install nginx -y
```

Create the page:

```bash
echo "Welcome to KKE Labs:Version 2" | sudo tee /var/www/html/index.html
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

Verify:

```bash
curl localhost
```

Expected:

```text
Welcome to KKE Labs:Version 2
```

<img width="472" height="227" alt="vm2 verify" src="https://github.com/user-attachments/assets/32878c2b-547a-47b6-a4e2-f64a7ed66ba8" />

# Part 4: Create the Application Gateway (UI)

Navigate to:

```text
Application Gateways
```

Click:

```text
+ Create
```

## Basics

| Setting | Value             |
| ------- | ----------------- |
| Name    | `datacenter-apgw` |
| Region  | `East US`         |
| Tier    | Standard V2       |

## Frontends

Create a new Public IP.

| Setting | Value                |
| ------- | -------------------- |
| Name    | `datacenter-apgw-ip` |

## Networking

Choose:

| Setting | Value                    |
| ------- | ------------------------ |
| VNet    | `datacenter-vnet`        |
| Subnet  | `datacenter-apgw-subnet` |

## Backend Pool

Click **Add Backend Pool**.

| Setting | Value                    |
| ------- | ------------------------ |
| Name    | `datacenter-backendpool` |

Add backend targets:

* `datacenter-vm1`
* `datacenter-vm2`

Save.

## Backend Settings

Click **Add Backend Setting**.

| Setting  | Value                      |
| -------- | -------------------------- |
| Name     | `datacenter-http-settings` |
| Protocol | HTTP                       |
| Port     | 80                         |

Save.

## Listener

Click **Add Listener**.

| Setting     | Value                 |
| ----------- | --------------------- |
| Name        | `datacenter-listener` |
| Frontend IP | Public                |
| Protocol    | HTTP                  |
| Port        | 80                    |

Save.

## Routing Rule

Click **Add Routing Rule**.

| Setting         | Value                      |
| --------------- | -------------------------- |
| Name            | `datacenter-routing-rule`  |
| Listener        | `datacenter-listener`      |
| Backend Pool    | `datacenter-backendpool`   |
| Backend Setting | `datacenter-http-settings` |

Save.

Click **Review + Create** → **Create**.

<img width="1107" height="437" alt="apigate" src="https://github.com/user-attachments/assets/4e7a2dd3-e7ee-4ee6-bf6f-816da085ef3f" />

> **Note:** Application Gateway deployment can take **5–10 minutes**.

# Part 5: Verify Load Balancing

After the Application Gateway deployment is complete:

Copy the public IP of:

```text
datacenter-apgw-ip
```

Open:

```text
http://<APPLICATION_GATEWAY_PUBLIC_IP>
```

Refresh the page multiple times.

You should see one of the following responses:

```text
Welcome to KKE Labs:Version 1
```

<img width="486" height="237" alt="apigatevm1" src="https://github.com/user-attachments/assets/3f1fcdcf-7997-41c9-a82b-e6518159327d" />

or

```text
Welcome to KKE Labs:Version 2
```

Depending on which backend VM handles the request.

You can also test with:

```bash
curl http://<APPLICATION_GATEWAY_PUBLIC_IP>
```

<img width="492" height="246" alt="apigatevm2" src="https://github.com/user-attachments/assets/f7b5000e-08bd-45a3-8b39-58ab5c4e5c4e" />

Run it several times to observe responses from both backend VMs.

## Fun Message

*"Your Azure Application Gateway is now balancing traffic across two Nginx servers, providing a highly available and scalable entry point for your web application 🚀🌐☁️"*
