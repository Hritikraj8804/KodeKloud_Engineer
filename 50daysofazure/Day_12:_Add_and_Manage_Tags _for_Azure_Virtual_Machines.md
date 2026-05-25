# Instruction

The Nautilus DevOps team is migrating a portion of their infrastructure to Azure. During the migration, they have created several virtual machines (VMs) in different regions. The team has identified one VM that is not tagged properly, so they decided to tag it as needed.

For this task:

* Add the following tag to the VM:

| Tag Name      | Value |
| ------------- | ----- |
| `Environment` | `dev` |

* Virtual Machine name:

```text id="k7m2xp"
xfusion-vm
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



## **2. Open Virtual Machines Service**

In the Azure Portal search bar, search for:

```text id="n8v3wa"
Virtual Machines
```

Click:

```text id="x5p1mr"
Virtual Machines
```

from the services list.



## **3. Open the Existing VM**

Search and open the VM:

```text id="c2t7yk"
xfusion-vm
```

<img width="1850" height="862" alt="vmlist" src="https://github.com/user-attachments/assets/ebc30e5c-9e1b-46eb-b387-658cb14df7a3" />


## **4. Open the Tags Section**

From the VM left-side menu:

Under:

```text id="w9m4qe"
Settings
```

Click:

```text id="b6x2nv"
Tags
```



## **5. Add the Required Tag**

Enter the following values:

| Field | Value         |
| ----- | ------------- |
| Name  | `Environment` |
| Value | `dev`         |

Then click:

```text id="r3k8yp"
Apply
```

or

```text id="m1v7zd"
Save
```

depending on the Azure Portal version.

<img width="998" height="557" alt="tags" src="https://github.com/user-attachments/assets/ebef2163-1de7-4fff-a930-50ae251375be" />


## **6. Verify the Tag**

After saving, verify the VM contains the tag:

| Tag Name      | Value |
| ------------- | ----- |
| `Environment` | `dev` |



## Tag Configuration Summary

| Resource  | Value         |
| --------- | ------------- |
| VM Name   | `xfusion-vm`  |
| Tag Name  | `Environment` |
| Tag Value | `dev`         |



## Challenges

### **1. Incorrect Tag Name**

Ensure the tag key is exactly:

```text id="p8q4lx"
Environment
```

Azure tags are case-sensitive.



### **2. Incorrect Tag Value**

Ensure the value is exactly:

```text id="t4p8qs"
dev
```



### **3. Forgetting to Save Changes**

Always click:

```text id="h2x6vn"
Save
```

or

```text id="f5n2wc"
Apply
```

after adding the tag.

<img width="998" height="557" alt="tags" src="https://github.com/user-attachments/assets/7fbf2a93-d46b-4bcb-8f51-17c01b331ebe" />


### **4. Editing Wrong Resource**

Make sure you open the VM:

```text id="y7r9mk"
xfusion-vm
```

before adding the tag.



## Fun Message

*"Your Azure VM is now properly labeled and organized like a true DevOps asset ☁️🏷️🚀"*
