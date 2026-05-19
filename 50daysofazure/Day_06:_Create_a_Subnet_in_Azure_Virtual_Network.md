# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.

For this task:

* Create a Virtual Network (VNet) named `devops-vnet`
* Create one subnet named `devops-subnet`
* Deploy them in the `southcentralus` region
* Use the IPv4 address range:

```text id="k7m2xp"
10.0.0.0/16
```

> **Note:** Use the Azure Portal (UI/Console) to complete this task.



# Solution

## **Steps to Complete the Task Using Azure Portal**

### **1. Login to Azure Portal**

Open:

```text id="u4q9lt"
https://portal.azure.com
```

Login using your Azure credentials.



## **2. Open Virtual Networks Service**

In the Azure Portal search bar, search for:

```text id="n8v3wa"
Virtual Networks
```

Click:

```text id="x5p1mr"
Virtual Networks
```

from the services list.



## **3. Create a New Virtual Network**

Click:

```text id="c2t7yk"
+ Create
```

Then select:

```text id="w9m4qe"
Virtual Network
```



## **4. Configure the Basics Tab**

Fill in the following details:

| Setting        | Value                          |
| -------------- | ------------------------------ |
| Resource Group | Select existing resource group |
| Name           | `devops-vnet`                  |
| Region         | `South Central US`             |

<img width="1037" height="735" alt="vnet" src="https://github.com/user-attachments/assets/3f42a1f4-0675-4d0f-aac1-cd3919c83eff" />


## **5. Configure IP Addresses**

Go to the:

```text id="b6x2nv"
IP Addresses
```

section.

Set the IPv4 address space to:

```text id="r3k8yp"
10.0.0.0/16
```



## **6. Create the Subnet**

Under the subnet section:

* Click:

```text id="m1v7zd"
+ Add subnet
```

Enter the following values:

| Setting              | Value           |
| -------------------- | --------------- |
| Subnet Name          | `devops-subnet` |
| Subnet Address Range | `10.0.0.0/24`   |

Then click:

```text id="p8q4lx"
Add
```

<img width="1362" height="522" alt="subnet" src="https://github.com/user-attachments/assets/389eeb1d-88e3-43c3-9bf9-5ab8b0c01205" />

<img width="1022" height="392" alt="ip" src="https://github.com/user-attachments/assets/034234da-f5d9-4f9b-9543-a3e0aa7cff4f" />

## **7. Keep Remaining Settings Default**

Leave:

* Security
* DNS
* Tags
* Peering

as default.



## **8. Review and Create**

Click:

```text id="t4p8qs"
Review + Create
```

<img width="657" height="747" alt="review" src="https://github.com/user-attachments/assets/63b81940-0ec8-44e7-8726-0b6d1a1f3b46" />


Once validation passes, click:

```text id="h2x6vn"
Create
```



## **9. Verify the Resources**

After deployment:

* Open:

```text id="f5n2wc"
Virtual Networks
```

* Search for:

```text id="y7r9mk"
devops-vnet
```

Verify:

* Region = `South Central US`
* Address Space = `10.0.0.0/16`
* Subnet = `devops-subnet`



## VNet Configuration Summary

| Setting       | Value              |
| ------------- | ------------------ |
| VNet Name     | `devops-vnet`      |
| Region        | `South Central US` |
| Address Space | `10.0.0.0/16`      |
| Subnet Name   | `devops-subnet`    |
| Subnet Range  | `10.0.0.0/24`      |



## Challenges

### **1. Incorrect Address Space**

Ensure the VNet IPv4 CIDR is exactly:

```text id="g1x7vq"
10.0.0.0/16
```



### **2. Invalid Subnet Range**

The subnet range must fall within the VNet range.

Example valid subnet:

```text id="d5m8ky"
10.0.0.0/24
```



### **3. Wrong Region Selection**

Verify the selected region is:

```text id="t2q4wc"
South Central US
```



### **4. Forgetting to Add the Subnet**

Ensure the subnet:

```text id="n6p3rt"
devops-subnet
```

is created before deployment.



## Fun Message

*"Your DevOps network is now connected and subnetted inside Azure cloud space ☁️🌐🚀"*
