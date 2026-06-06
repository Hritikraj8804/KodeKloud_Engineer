# Instruction

The Nautilus DevOps team needs to set up a new Azure Virtual Machine that can be accessed securely from the `azure-client` host using SSH key authentication.

### Requirements

| Setting        | Value                                 |
| -------------- | ------------------------------------- |
| VM Name        | `devops-xm`                           |
| Region         | `westus`                              |
| VM Size        | `Standard_B1s`                        |
| Username       | `azureuser`                           |
| Authentication | SSH Public Key                        |
| Access         | Password-less SSH from `azure-client` |

> **Note:** Use the Azure Portal (UI) to create the VM. SSH verification will be performed from the `azure-client` host.



# Solution

## 1. Check for Existing SSH Key on azure-client

Login to the `azure-client` host and run:

```bash
ls -la ~/.ssh/
```

Look for:

```text
id_rsa
id_rsa.pub
```

If they already exist, reuse them.

If not, create a new key pair:

```bash
ssh-keygen -t rsa -f ~/.ssh/id_rsa -N ""
```

Verify:

```bash
cat ~/.ssh/id_rsa.pub
```

Copy the entire public key.



## 2. Login to Azure Portal

Open:

```text
https://portal.azure.com
```

Login with the lab credentials.



## 3. Create the Virtual Machine

Search:

```text
Virtual Machines
```

Click:

```text
+ Create → Azure Virtual Machine
```



## 4. Configure the Basics Tab

Enter:

| Setting              | Value                                 |
| -------------------- | ------------------------------------- |
| Resource Group       | Existing lab resource group           |
| Virtual Machine Name | `devops-xm`                           |
| Region               | `West US`                             |
| Image                | Ubuntu Server (any available version) |
| Size                 | `Standard_B1s`                        |



## 5. Configure Administrator Account

Under **Administrator Account**:

Select:

```text
SSH public key
```

Set:

| Field                 | Value                                 |
| --------------------- | ------------------------------------- |
| Username              | `azureuser`                           |
| SSH Public Key Source | Use existing public key               |
| Public Key            | Paste contents of `~/.ssh/id_rsa.pub` |

Example:

```text
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQ...
```



## 6. Configure Inbound Ports

Select:

```text
Allow selected ports
```

Choose:

```text
SSH (22)
```

This automatically creates the required NSG rule.



## 7. Create the VM

Click:

```text
Review + Create
```

<img width="740" height="832" alt="vmreview" src="https://github.com/user-attachments/assets/54a5f43f-0b7b-471c-b492-23de77603910" />

After validation succeeds:

```text
Create
```

Wait for deployment to finish.



## 8. Obtain the Public IP

Open:

```text
devops-xm
```

From the Overview page, copy:

```text
Public IP Address
```

Example:

```text
20.xx.xx.xx
```

<img width="1167" height="660" alt="vm" src="https://github.com/user-attachments/assets/173b3a5e-947f-4b82-8933-bfd6544362fd" />


## 9. Verify Password-less SSH Access

From the `azure-client` host:

```bash
ssh azureuser@<PUBLIC_IP>
```

Example:

```bash
ssh azureuser@20.xx.xx.xx
```

If prompted:

```text
Are you sure you want to continue connecting (yes/no)?
```

Type:

```text
yes
```

You should log in directly without being asked for a password.

Expected:

```text
Welcome to Ubuntu
```

<img width="622" height="675" alt="ssh" src="https://github.com/user-attachments/assets/fe09f96c-f5a6-44b2-85c2-3d20b615bcc7" />

<img width="392" height="137" alt="whoami" src="https://github.com/user-attachments/assets/d6c07325-3e83-440f-bff7-f316f1e622fe" />

## Fun Message

*"Your DevOps VM is now securely connected to the azure-client host with password-less SSH access ☁️🔐🚀"*
