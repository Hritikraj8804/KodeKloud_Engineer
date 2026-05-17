# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.

Create a Virtual Network (VNet) named `xfusion-vnet` in the `southcentralus` region with any IPv4 CIDR block.

> **Note:** Use the Azure Portal (Console) to complete this task.



# Solution

## **Steps to Complete the Task Using Azure Console**

### **1. Login to Azure Portal**

Open:

```text id="k7m2xp"
https://portal.azure.com
```

Login with your Azure account credentials.



## **2. Open Virtual Networks Service**

In the Azure Portal search bar, search for:

```text id="u4q9lt"
Virtual Networks
```

Click:

```text id="n8v3wa"
Virtual Networks
```

from the services list.



## **3. Create a New Virtual Network**

Click:

```text id="x5p1mr"
+ Create
```

<img width="1627" height="742" alt="VNet" src="https://github.com/user-attachments/assets/b2c53066-d402-4813-817c-ffcfe35a1ea6" />


Then select:

```text id="c2t7yk"
Virtual Network
```



## **4. Configure the Basics Tab**

Fill in the required details:

| Setting        | Value                          |
| -------------- | ------------------------------ |
| Resource Group | Select existing resource group |
| Name           | `xfusion-vnet`                 |
| Region         | `South Central US`             |

<img width="992" height="720" alt="form" src="https://github.com/user-attachments/assets/01a88d62-44c9-4f3e-9398-ffd4054d414d" />


## **5. Configure IP Addresses**

Under the:

```text id="w9m4qe"
IP Addresses
```

section, enter any valid IPv4 CIDR block.

Example:

```text id="b6x2nv"
10.0.0.0/16
```

<img width="960" height="717" alt="add" src="https://github.com/user-attachments/assets/e2ae75be-f602-44d2-b478-1496ba634ea9" />


You can keep the default subnet or create a custom subnet.

Example subnet:

```text id="r3k8yp"
10.0.0.0/24
```



## **6. Keep Remaining Settings Default**

Leave:

* Security
* Tags
* DNS
* Peering

as default.



## **7. Review and Create**

Click:

```text id="m1v7zd"
Review + Create
```

<img width="677" height="735" alt="review" src="https://github.com/user-attachments/assets/89260a4a-b406-4f5c-9552-9b09c7432675" />


After validation succeeds, click:

```text id="p8q4lx"
Create
```



## **8. Verify the Virtual Network**

After deployment:

* Open:

```text id="f5n2wc"
Virtual Networks
```

* Search for:

```text id="y7r9mk"
xfusion-vnet
```

<img width="1367" height="277" alt="list" src="https://github.com/user-attachments/assets/b0861f01-5fe5-4255-88bf-ac5b41d9d223" />


You should see the VNet successfully created.

<img width="1015" height="211" alt="cli list" src="https://github.com/user-attachments/assets/f3ce7b04-8339-425e-84cf-affb46775f6f" />


## VNet Configuration Summary

| Setting       | Value                     |
| ------------- | ------------------------- |
| VNet Name     | `xfusion-vnet`            |
| Region        | `South Central US`        |
| Address Space | `10.0.0.0/16` *(example)* |
| Subnet        | `10.0.0.0/24` *(example)* |



## Challenges

### **1. Incorrect Region Selection**

Ensure the region is:

```text id="t4p8qs"
South Central US
```

Azure resources are region-specific.



### **2. Invalid CIDR Block**

Use a valid private IPv4 range such as:

```text id="h2x6vn"
10.0.0.0/16
```

Avoid overlapping address ranges.



### **3. Wrong Resource Group**

Always verify the correct existing resource group is selected before deployment.



## Fun Message

*"Your xFusion network is now connected across the Azure cloud highways ☁️🌐🚀"*
