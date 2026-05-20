# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.

For this task:

* Allocate a Public IP address
* Name it:

```text id="k7m2xp"
devops-pip
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



## **2. Open Public IP Addresses Service**

In the Azure Portal search bar, search for:

```text id="n8v3wa"
Public IP addresses
```

Click:

```text id="x5p1mr"
Public IP addresses
```

from the services list.



## **3. Create a New Public IP Address**

Click:

```text id="c2t7yk"
+ Create
```



## **4. Configure the Basics Tab**

Fill in the following details:

| Setting        | Value                              |
| -------------- | ---------------------------------- |
| Resource Group | Select existing resource group     |
| Name           | `devops-pip`                       |
| Region         | Select the required/default region |
| IP Version     | `IPv4`                             |
| SKU            | `Basic`                            |
| Assignment     | `Dynamic`                          |

Keep remaining settings as default.



## **5. Review and Create**

Click:

```text id="w9m4qe"
Review + Create
```

<img width="580" height="745" alt="review" src="https://github.com/user-attachments/assets/89bf4fcd-c7bd-4a6a-b510-26bb3ecb5653" />


After validation succeeds, click:

```text id="b6x2nv"
Create
```



## **6. Verify the Public IP Address**

After deployment:

* Open:

```text id="r3k8yp"
Public IP addresses
```

* Search for:

```text id="m1v7zd"
devops-pip
```

Verify the Public IP resource has been created successfully.



## Public IP Configuration Summary

| Setting        | Value        |
| -------------- | ------------ |
| Public IP Name | `devops-pip` |
| IP Version     | `IPv4`       |
| SKU            | `Basic`      |
| Assignment     | `Dynamic`    |



## Challenges

### **1. Incorrect Resource Name**

Ensure the Public IP name is exactly:

```text id="p8q4lx"
devops-pip
```



### **2. Wrong IP Version**

Verify the IP version selected is:

```text id="t4p8qs"
IPv4
```



### **3. SKU Selection Issues**

If not specified in the task, keep:

```text id="h2x6vn"
Basic
```

as default.



### **4. Region Selection**

Choose the same region as other related Azure resources when possible.



## Fun Message

*"Your Azure public gateway is now live on the internet highways ☁️🌍🚀"*
