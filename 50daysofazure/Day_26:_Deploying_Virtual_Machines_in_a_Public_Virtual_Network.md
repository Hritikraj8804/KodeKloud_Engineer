# Instruction

The Nautilus DevOps Team has received a request from the Networking Team to set up a new public VNet to support public-facing services.

### Requirements

#### Virtual Network

| Resource | Name                 |
| -------- | -------------------- |
| VNet     | `xfusion-pub-vnet`   |
| Subnet   | `xfusion-pub-subnet` |

#### Virtual Machine

| Resource | Name             |
| -------- | ---------------- |
| VM       | `xfusion-pub-vm` |

### Additional Requirements

* The VM must be deployed inside `xfusion-pub-vnet`.
* The subnet should support public internet access.
* The VM must receive a Public IP.
* SSH Port `22` must be open from the internet.
* SSH access must work successfully.

> **Note:** Use the Azure Portal (UI) to complete the task.



# Solution

## 1. Login to Azure Portal

Open:

```text
https://portal.azure.com
```

Login with the lab credentials.



# Part 1: Create the Public VNet

## 2. Create a Virtual Network

Search:

```text
Virtual Networks
```

Click:

```text
+ Create
```



## 3. Configure the VNet

### Basics Tab

| Setting        | Value                       |
| -------------- | --------------------------- |
| Resource Group | Existing lab resource group |
| Name           | `xfusion-pub-vnet`          |
| Region         | Use the lab/default region  |

Click:

```text
Next : IP Addresses
```



## 4. Configure Address Space

Use any valid private CIDR.

Example:

```text
10.0.0.0/16
```



## 5. Create the Subnet

Click:

```text
+ Add subnet
```

Configure:

| Setting       | Value                |
| ------------- | -------------------- |
| Subnet Name   | `xfusion-pub-subnet` |
| Address Range | `10.0.0.0/24`        |

Click:

```text
Add
```

<img width="1032" height="555" alt="subnet" src="https://github.com/user-attachments/assets/ebd9f638-6626-4d23-a7d2-688da583d073" />

Then:

```text
Review + Create
```

<img width="627" height="772" alt="review" src="https://github.com/user-attachments/assets/45394b62-52a1-4fa4-9366-bbf3cd542db8" />

Finally:

```text
Create
```



# Part 2: Create the Public VM

## 6. Create a Virtual Machine

Search:

```text
Virtual Machines
```

Click:

```text
+ Create → Azure Virtual Machine
```



## 7. Configure the VM

### Basics Tab

| Setting        | Value                       |
| -------------- | --------------------------- |
| Resource Group | Existing lab resource group |
| VM Name        | `xfusion-pub-vm`            |
| Image          | Any Ubuntu image            |
| Size           | `Standard_B1s`              |
| Authentication | SSH Public Key              |
| Username       | `azureuser`                 |

Generate a new key pair or use an existing one.



## 8. Configure Networking

### Networking Tab

Select:

| Setting         | Value                |
| --------------- | -------------------- |
| Virtual Network | `xfusion-pub-vnet`   |
| Subnet          | `xfusion-pub-subnet` |

For Public IP:

```text
Create New
```

Leave:

```text
Public IP = Enabled
```

This ensures the VM receives a public IP address.



## 9. Open SSH Access

Under:

```text
Inbound Ports
```

Select:

```text
Allow selected ports
```

Choose:

```text
SSH (22)
```

Azure automatically creates the NSG rule.



## 10. Create the VM

Click:

```text
Review + Create
```

<img width="537" height="797" alt="vm review" src="https://github.com/user-attachments/assets/c755f9fb-b363-44a2-ae96-d763c28a15f1" />

After validation succeeds:

```text
Create
```

Download the SSH private key if Azure generates one.

Wait for deployment to finish.

<img width="887" height="617" alt="vm" src="https://github.com/user-attachments/assets/f6667094-2a59-4ab2-b41c-548c5ab320eb" />


# Part 3: Verify Public Access

## 11. Get the Public IP

Open:

```text
xfusion-pub-vm
```

Copy:

```text
Public IP Address
```

Example:

```text
20.xx.xx.xx
```



## 12. Verify NSG Rule

Navigate:

```text
Networking
```

Confirm an inbound rule exists:

| Protocol | Port |
| -------- | ---- |
| TCP      | 22   |

Source:

```text
Any
```

Action:

```text
Allow
```



## 13. Test SSH Connectivity

From your terminal:

```bash
ssh azureuser@<PUBLIC_IP>
```

Example:

```bash
ssh azureuser@20.xx.xx.xx
```

Expected:

```text
Welcome to Ubuntu
```


## Fun Message

*"Your public Azure network is ready, and xfusion-pub-vm is now reachable from the internet via SSH ☁️🌐🚀"*
