# Instruction

The Nautilus DevOps team needs a **private Azure network** where resources can communicate only within the VNet.

### Requirements

#### Virtual Network

| Resource      | Name                     |
| ------------- | ------------------------ |
| VNet          | `datacenter-priv-vnet`   |
| Subnet        | `datacenter-priv-subnet` |
| Address Space | `10.0.0.0/16`            |
| Region        | `Central US`             |

#### Network Security Group

| Resource | Name                  |
| -------- | --------------------- |
| NSG      | `datacenter-priv-nsg` |

#### Virtual Machine

| Resource | Name                 |
| -------- | -------------------- |
| VM       | `datacenter-priv-vm` |
| Region   | `Central US`         |

#### Required NSG Rule

| Setting     | Value         |
| ----------- | ------------- |
| Source      | `10.0.0.0/16` |
| Destination | `10.0.0.0/16` |
| Protocol    | TCP           |
| Port        | 22            |
| Action      | Allow         |

> **Important:** Do NOT assign a Public IP to the VM. This is a private VNet setup.



# Step 1: Login to Azure Portal

Open:

```text
https://portal.azure.com
```

Login using the lab credentials.



# Step 2: Create the Private VNet

Search:

```text
Virtual Networks
```

Click:

```text
+ Create
```

### Basics

| Setting        | Value                       |
| -------------- | --------------------------- |
| Resource Group | Existing lab resource group |
| Name           | `datacenter-priv-vnet`      |
| Region         | `Central US`                |

Click:

```text
Next : IP Addresses
```

### IP Addresses

Address Space:

```text
10.0.0.0/16
```

Delete the default subnet and create:

| Setting      | Value                    |
| ------------ | ------------------------ |
| Subnet Name  | `datacenter-priv-subnet` |
| Subnet Range | `10.0.0.0/24`            |

Click:

```text
Add
```

Then:

```text
Review + Create
```

Finally:

```text
Create
```

<img width="652" height="777" alt="vnet" src="https://github.com/user-attachments/assets/89ab7434-4484-47ae-8a23-2affb6b9d963" />

# Step 3: Create the NSG

Search:

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
| Name           | `datacenter-priv-nsg`       |
| Region         | `Central US`                |
| Resource Group | Existing lab resource group |

Click:

```text
Review + Create
```

Then:

```text
Create
```

<img width="526" height="325" alt="sg" src="https://github.com/user-attachments/assets/d61a0e14-f670-4aea-bf35-976d7af00d20" />

# Step 4: Create the SSH Rule

Open:

```text
datacenter-priv-nsg
```

Navigate:

```text
Inbound Security Rules
```

Click:

```text
+ Add
```

Configure:

| Setting             | Value                |
| ------------------- | -------------------- |
| Source              | IP Addresses         |
| Source IP/CIDR      | `10.0.0.0/16`        |
| Source Port         | *                    |
| Destination         | IP Addresses         |
| Destination IP/CIDR | `10.0.0.0/16`        |
| Destination Port    | `22`                 |
| Protocol            | TCP                  |
| Action              | Allow                |
| Priority            | `1000`               |
| Name                | `Allow-SSH-Internal` |

Click:

```text
Add
```

<img width="570" height="831" alt="inboundsg" src="https://github.com/user-attachments/assets/757a5040-73c3-412d-89c0-de245971ce12" />

# Step 5: Associate NSG with the Subnet

Open:

```text
datacenter-priv-vnet
```

Navigate:

```text
Subnets
```

Open:

```text
datacenter-priv-subnet
```

Under:

```text
Network Security Group
```

Select:

```text
datacenter-priv-nsg
```

Click:

```text
Save
```

<img width="1907" height="852" alt="assocaition" src="https://github.com/user-attachments/assets/43a10706-3855-451e-9ba8-159a9a4d65b2" />

# Step 6: Create the Private VM

Search:

```text
Virtual Machines
```

Click:

```text
+ Create → Azure Virtual Machine
```



## Basics

| Setting        | Value                          |
| -------------- | ------------------------------ |
| VM Name        | `datacenter-priv-vm`           |
| Region         | `Central US`                   |
| Image          | Ubuntu (any available version) |
| Size           | `Standard_B1s`                 |
| Username       | `azureuser`                    |
| Authentication | SSH Public Key                 |

Generate a new key pair or use an existing key.



## Networking Tab

Select:

| Setting         | Value                    |
| --------------- | ------------------------ |
| Virtual Network | `datacenter-priv-vnet`   |
| Subnet          | `datacenter-priv-subnet` |

### Critical Setting

For Public IP:

```text
None
```

or

```text
Disable Public IP
```

This keeps the VM private.

For NSG:

```text
Use existing
```

Select:

```text
datacenter-priv-nsg
```

Do not enable public inbound ports.



## Create VM

Click:

```text
Review + Create
```

<img width="457" height="832" alt="vm review" src="https://github.com/user-attachments/assets/5c00045f-969c-407e-b2d3-f79b5373563c" />

Then:

```text
Create
```

<img width="1662" height="840" alt="vm" src="https://github.com/user-attachments/assets/dacacc04-846e-40e9-8536-82cb38c6bc38" />
