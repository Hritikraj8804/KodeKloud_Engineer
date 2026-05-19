# Instruction

The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the Azure cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Networks (VNets) as the initial step, as they will be provisioning various services under different VNets.

Create a Virtual Network (VNet) named `nautilus-vnet` in the `eastus` region with the IPv4 CIDR block:

```text id="k4m8xp"
192.168.0.0/24
```

Use the Azure credentials provided on the `azure-client` host.

> **Note:** Run the following command on the `azure-client` host to retrieve Azure credentials:

```bash id="u7q2lv"
showcreds
```

Use the Azure Portal (Console) to complete this task.



# Solution

## **Steps to Complete the Task Using Azure Console**

### **1. Retrieve Azure Credentials**

Login to the `azure-client` host and run:

```bash id="r3x9nt"
showcreds
```

Copy the:

* Azure Username
* Password
* Subscription Details



## **2. Login to Azure Portal**

Open:

```text id="m8v4pk"
https://portal.azure.com
```

Login using the credentials retrieved from the `showcreds` command.



## **3. Open Virtual Networks Service**

In the Azure Portal search bar, search for:

```text id="p5n1wc"
Virtual Networks
```

Click:

```text id="f2k7ya"
Virtual Networks
```

from the services list.



## **4. Create a New Virtual Network**

Click:

```text id="x6m3qt"
+ Create
```

Then select:

```text id="w9p8lr"
Virtual Network
```



## **5. Configure the Basics Tab**

Fill in the required details:

| Setting        | Value                          |
| -------------- | ------------------------------ |
| Resource Group | Select existing resource group |
| Name           | `nautilus-vnet`                |
| Region         | `East US`                      |

<img width="1077" height="725" alt="vnetname" src="https://github.com/user-attachments/assets/fbe9c4e1-3c9e-4fc7-b14b-37344b89a996" />


## **6. Configure IP Addresses**

Under the:

```text id="c4t7yn"
IP Addresses
```

section, configure the address space:

| Setting            | Value            |
| ------------------ | ---------------- |
| IPv4 address space | `192.168.0.0/24` |

You may keep the default subnet or create a subnet manually.

Example subnet:

```text id="q2v6mk"
192.168.0.0/25
```

<img width="1011" height="461" alt="sunet" src="https://github.com/user-attachments/assets/466b0287-4444-49a6-8a4b-a3530a2fba7b" />


## **7. Keep Remaining Settings Default**

Leave:

* Security
* DNS
* Tags
* Peering

as default.



## **8. Review and Create**

Click:

```text id="z5x1wp"
Review + Create
```

<img width="720" height="732" alt="review" src="https://github.com/user-attachments/assets/9db4076d-f8bf-4a1e-abbe-6f0acc969134" />

After successful validation, click:

```text id="n7m4qe"
Create
```



## **9. Verify the Virtual Network**

After deployment:

* Open:

```text id="h3p9kt"
Virtual Networks
```

* Search for:

```text id="y8q2lv"
nautilus-vnet
```

Verify:

* Region = `East US`
* Address Space = `192.168.0.0/24`



## VNet Configuration Summary

| Setting       | Value            |
| ------------- | ---------------- |
| VNet Name     | `nautilus-vnet`  |
| Region        | `East US`        |
| Address Space | `192.168.0.0/24` |

<img width="1057" height="572" alt="res" src="https://github.com/user-attachments/assets/18bc38e5-bf81-468b-8789-17154ab3787e" />


## Challenges

### **1. Incorrect CIDR Block**

Ensure the IPv4 range is exactly:

```text id="b1m6xz"
192.168.0.0/24
```



### **2. Wrong Region Selection**

Verify the region is:

```text id="d9q4vt"
East US
```

Azure resources are region-specific.



### **3. Incorrect Resource Group**

Always select the existing resource group provided in the lab environment.



### **4. Portal Login Failure**

If login fails, rerun:

```bash id="g6p2wy"
showcreds
```

to verify the credentials.



## Fun Message

*"The Nautilus network has now anchored safely in Azure East US waters ☁️🌐⚓"*
