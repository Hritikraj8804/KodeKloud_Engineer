# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.

For this task, create a **Managed Disk** with the following requirements:

| Setting   | Value          |
| --------- | -------------- |
| Disk Name | `devops-disk`  |
| Disk Type | `Standard_LRS` |
| Disk Size | `2 GiB`        |

> **Note:** Use the Azure Portal (UI/Console) to complete this task.



# Solution

## **Steps to Complete the Task Using Azure Portal**

### **1. Login to Azure Portal**

Open:

```text
https://portal.azure.com
```

Login using your Azure credentials.



## **2. Open Disks Service**

In the Azure Portal search bar, search for:

```text
Disks
```

Click:

```text
Disks
```

from the services list.



## **3. Create a New Managed Disk**

Click:

```text
+ Create
```



## **4. Configure the Basics Tab**

Fill in the following details:

| Setting        | Value                          |
| -------------- | ------------------------------ |
| Resource Group | Select existing resource group |
| Disk Name      | `devops-disk`                  |
| Region         | Select the lab/default region  |
| Source Type    | `None (empty disk)`            |



## **5. Configure Size and Performance**

Under **Size + Performance**:

Set:

| Setting      | Value                |
| ------------ | -------------------- |
| Storage Type | `Standard HDD (LRS)` |
| Disk Size    | `2 GiB`              |

> Azure may display this as **Standard HDD (LRS)** in the portal, which corresponds to `Standard_LRS`.



## **6. Review and Create**

Click:

```text
Review + Create
```

After validation succeeds, click:

```text
Create
```

<img width="1002" height="827" alt="config" src="https://github.com/user-attachments/assets/fcd5364a-6358-48dd-b399-82fd535048bd" />


## **7. Verify the Disk**

After deployment:

1. Open:

```text
Disks
```

2. Search for:

```text
devops-disk
```

3. Verify:

| Property     | Expected Value       |
| ------------ | -------------------- |
| Name         | `devops-disk`        |
| Size         | `2 GiB`              |
| Storage Type | `Standard HDD (LRS)` |



# Managed Disk Configuration Summary

| Setting   | Value          |
| --------- | -------------- |
| Disk Name | `devops-disk`  |
| Disk Type | `Standard_LRS` |
| Disk Size | `2 GiB`        |
| Source    | Empty Disk     |

<img width="1696" height="571" alt="disk" src="https://github.com/user-attachments/assets/113c3612-f52a-462f-b8b3-39250c829622" />


# Challenges

### **1. Wrong Storage Type**

Make sure you select:

```text
Standard HDD (LRS)
```

and not:

```text
Premium SSD
```

or

```text
Standard SSD
```



### **2. Incorrect Disk Size**

Ensure the size is exactly:

```text
2 GiB
```



### **3. Selecting Wrong Source**

For this task, choose:

```text
None (empty disk)
```

as the source type.



### **4. Resource Group Selection**

Use the existing resource group provided by the lab environment.



## Fun Message

*"Your Azure storage fleet just gained a brand-new managed disk ready for deployment ☁️💽🚀"*
