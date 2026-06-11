# Instruction

The Nautilus DevOps Team attempted to deploy an Nginx server on an Azure VM, but the server is not accessible from the internet.

### Existing Resources

| Resource  | Name           |
| --------- | -------------- |
| VNet      | `xfusion-vnet` |
| VM        | `xfusion-vm`   |
| Public IP | `xfusion-pip`  |
| Region    | `West US`      |

### Tasks

1. Verify `xfusion-vnet` is configured for internet access.
2. Attach the existing Public IP `xfusion-pip` to `xfusion-vm`.
3. Ensure HTTP traffic (Port 80) is allowed.
4. Install Nginx on `xfusion-vm`.
5. Ensure Nginx is enabled and running.
6. Verify the VM is reachable from the internet on port 80.

> **Note:** Use the Azure Portal (UI) for Azure configuration and SSH into the VM for Nginx installation.



# Part 1: Attach the Public IP

## Step 1: Login to Azure Portal

Open:

```text
https://portal.azure.com
```

Login with the lab credentials.



## Step 2: Open the VM

Search:

```text
Virtual Machines
```

Open:

```text
xfusion-vm
```

## Step 3: Open the VM NIC

Navigate:

```text
Networking
```

Click the Network Interface attached to the VM.

<img width="1077" height="600" alt="before vnet" src="https://github.com/user-attachments/assets/5dcb5595-7003-4fde-8560-5fe4be39373b" />

## Step 4: Configure IP Settings

Inside the NIC:

```text
Settings → IP Configurations
```

Open:

```text
ipconfig1
```

Under:

```text
Public IP Address
```

Select:

```text
Associate
```

Choose:

```text
xfusion-pip
```

Click:

```text
Save
```

<img width="1452" height="832" alt="ipassocation" src="https://github.com/user-attachments/assets/8ebca86f-35a3-4620-b43f-ddb79bf60238" />

# Part 2: Verify NSG Configuration

## Step 5: Open Networking

Return to:

```text
xfusion-vm
```

Navigate:

```text
Networking
```

Verify an NSG is attached either:

* To the NIC
* Or to the subnet



## Step 6: Add HTTP Rule

Under:

```text
Inbound Port Rules
```

Click:

```text
+ Add inbound port rule
```

Configure:

| Setting          | Value        |
| ---------------- | ------------ |
| Source           | Any          |
| Source Port      | *            |
| Destination      | Any          |
| Destination Port | `80`         |
| Protocol         | TCP          |
| Action           | Allow        |
| Priority         | `1000`       |
| Name             | `Allow-HTTP` |

Click:

```text
Add
```



# Part 3: Verify VM Has Public Connectivity

## Step 7: Get Public IP

Open:

```text
xfusion-pip
```

Copy the assigned Public IP address.

Example:

```text
20.xx.xx.xx
```



# Part 4: Install Nginx

## Step 8: Connect to the VM

From Azure Portal:

```text
VM → Connect → SSH
```

Or from the `azure-client` host:

```bash
ssh azureuser@<PUBLIC_IP>
```

Example:

```bash
ssh azureuser@20.xx.xx.xx
```



## Step 9: Install Nginx

Ubuntu:

```bash
sudo apt update
sudo apt install nginx -y
```

<img width="657" height="126" alt="nginx i" src="https://github.com/user-attachments/assets/46d1876e-c8a1-40e4-a5ec-ac8c66a84ab1" />

## Step 10: Enable and Start Nginx

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```
<img width="847" height="92" alt="nginx start" src="https://github.com/user-attachments/assets/8144ec18-a698-4439-8e2a-3632854bd01b" />

Verify:

```bash
sudo systemctl status nginx
```

Expected:

```text
active (running)
```



## Step 11: Verify Port 80

Inside the VM:

```bash
sudo ss -tlnp | grep :80
```

Expected:

```text
LISTEN
```

on port:

```text
80
```



# Part 5: Verify Public Access

From the `azure-client` host:

```bash
curl http://<PUBLIC_IP>
```

Example:

```bash
curl http://20.xx.xx.xx
```

Expected output:

```html
Welcome to nginx!
```

<img width="636" height="510" alt="nginx running" src="https://github.com/user-attachments/assets/9ff2145c-f699-4386-8b6a-365bdebd6ec1" />

# Verification Checklist

### Networking

✅ VNet:

```text
xfusion-vnet
```

exists

✅ Public IP:

```text
xfusion-pip
```

attached to VM NIC



### NSG

✅ NSG attached

✅ HTTP rule:

```text
TCP 80
Allow
Source: Any
```

present



### VM

✅ VM:

```text
xfusion-vm
```

running



### Nginx

✅ Installed

✅ Enabled

```bash
systemctl enable nginx
```

✅ Running

```bash
systemctl status nginx
```



### Internet Access

✅ Public IP reachable

✅

```bash
curl http://<PUBLIC_IP>
```

returns Nginx page


