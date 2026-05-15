# Instruction

The Nautilus DevOps team is planning to migrate a portion of their infrastructure to the Azure cloud incrementally. As part of this migration, you are tasked with creating an Azure Virtual Machine (VM). The requirements are:

* Use the existing resource group.
* The VM name must be `datacenter-vm`.
* The VM should be created in the `southcentralus` region.
* Use the `Ubuntu 24.04 LTS` image for the VM.
* The VM size must be `Standard_B1s`.
* Attach a default Network Security Group (NSG) that allows inbound SSH (`port 22`).
* Attach a `30 GB` storage disk of type `Standard HDD`.
* The rest of the configurations should remain default.
* After completing the setup, ensure you can SSH into the virtual machine.

> **Note:** Use the Azure Portal (Console) to complete this task.



# Solution

## **Steps to Complete the Task Using Azure Console**

### **1. Login to Azure Portal**

Open:

```text id="w4q8lp"
https://portal.azure.com
```

Login with your Azure account credentials.



### **2. Open Virtual Machines Service**

In the Azure Portal:

* Search for:

```text id="x2m6vt"
Virtual Machines
```

* Click:

```text id="n7k3wa"
Virtual Machines
```

from the services list.

<img width="1661" height="787" alt="vm page" src="https://github.com/user-attachments/assets/02c9fb89-8874-48b3-aeb6-17b62b61855e" />


### **3. Create a New Virtual Machine**

Click:

```text id="c5r9ye"
+ Create
```

Then select:

```text id="h1p4zs"
Azure Virtual Machine
```



## **4. Configure the Basics Tab**

Fill in the following details:

| Setting              | Value                          |
| -------------------- | ------------------------------ |
| Resource Group       | Select existing resource group |
| Virtual Machine Name | `datacenter-vm`                |
| Region               | `South Central US`             |
| Image                | `Ubuntu Server 24.04 LTS`      |
| Size                 | `Standard_B1s`                 |



## **5. Configure Administrator Account**

Under:

```text id="v8m2xk"
Administrator account
```

Select:

```text id="a4q7nt"
SSH public key
```

Then:

* Username:

```text id="b9r5wc"
azureuser
```

* Generate a new key pair.

Keep other settings as default.



## **6. Configure Inbound Ports**

Under:

```text id="q3n8lv"
Inbound port rules
```

Select:

```text id="k7p1mf"
Allow selected ports
```

Then choose:

```text id="z2x6ta"
SSH (22)
```

This automatically creates and attaches a default NSG allowing SSH access.



## **7. Configure Disk Settings**

Go to the:

```text id="m4w9pk"
Disks
```

tab.

Set:

| Setting      | Value                |
| ------------ | -------------------- |
| OS Disk Size | `30 GiB`             |
| OS Disk Type | `Standard HDD (LRS)` |

Keep remaining options as default.



## **8. Review and Create**

Click:

```text id="r6v2yb"
Review + Create
```

Azure will validate the configuration.

If validation passes, click:

```text id="p1n7cq"
Create
```



## **9. Download the SSH Private Key**

Azure will automatically generate a key pair.

Download the private key file:

```text id="j5t8wd"
.pem
```

Store it securely.



## **10. Get the Public IP Address**

After deployment:

* Open the VM:

```text id="f2m4zr"
datacenter-vm
```

* Copy the:

```text id="y8k1ns"
Public IP address
```

from the Overview section.

<img width="1137" height="717" alt="vm" src="https://github.com/user-attachments/assets/959f467c-90e8-43c7-9090-4a4bee05550d" />



## **11. Connect to the VM Using SSH**

Open terminal and run:

```bash id="u3q7xe"
ssh -i <PRIVATE_KEY>.pem azureuser@<PUBLIC_IP>
```

Example:

```bash id="l9v5pt"
ssh -i nautilus-kp.pem azureuser@20.xx.xx.xx
```

<img width="797" height="937" alt="ssh" src="https://github.com/user-attachments/assets/fed60fd4-7949-4ce9-80f0-39d9966f1f98" />


If prompted:

```text id="s4n8yc"
Are you sure you want to continue connecting (yes/no)?
```

Type:

```text id="e7m2qa"
yes
```



## Azure Resources Created

| Resource               | Purpose                      |
| ---------------------- | ---------------------------- |
| Virtual Machine        | Compute instance             |
| Network Security Group | Allows SSH access            |
| Public IP Address      | Enables external access      |
| Managed Disk           | Stores operating system data |

---

## VM Configuration Summary

| Setting   | Value                     |
| --------- | ------------------------- |
| VM Name   | `datacenter-vm`           |
| Region    | `South Central US`        |
| Image     | `Ubuntu Server 24.04 LTS` |
| Size      | `Standard_B1s`            |
| Disk Size | `30 GB`                   |
| Disk Type | `Standard HDD`            |
| SSH Port  | `22`                      |

---

## Challenges

### **1. Wrong VM Size**

Ensure you select:

```text id="n6p3rt"
Standard_B1s
```

Other VM sizes may fail task validation.

---

### **2. Incorrect Region**

Verify the selected region is:

```text id="g1x7vq"
South Central US
```

---

### **3. Forgetting SSH Port**

If SSH port `22` is not enabled, remote login will fail.

Always select:

```text id="d5m8ky"
Allow selected ports → SSH (22)
```



### **4. Losing the Private Key**

Download and securely save the:

```text id="t2q4wc"
.pem
```

file immediately after VM creation.



## Fun Message

*"Your Nautilus VM has officially docked in Azure Harbor ☁️🚢🔐"*
