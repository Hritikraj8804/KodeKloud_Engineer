# Instruction

As part of the data migration process, the Nautilus DevOps team is actively creating several storage containers on Azure. They plan to utilize **private Blob containers** to store relevant data.

For this task:

1. Create a **Storage Account** named:

```text
xfusionst30191
```

2. Create a **Blob Container** named:

```text
xfusion-blob-12846
```

3. Ensure the Blob Container access level is:

```text
Private (no anonymous access)
```

> **Note:** Use the Azure Portal (UI/Console) to complete this task.



# Solution

## **1. Login to Azure Portal**

Open:

```text
https://portal.azure.com
```

Login using your Azure credentials.



## **2. Create the Storage Account**

In the Azure Portal search bar, search for:

```text
Storage Accounts
```

Click:

```text
Storage Accounts
```

Then click:

```text
+ Create
```



## **3. Configure the Storage Account**

Fill in the following details:

| Setting              | Value                           |
| -------------------- | ------------------------------- |
| Resource Group       | Select existing resource group  |
| Storage Account Name | `xfusionst30191`                |
| Region               | Select the lab/default region   |
| Performance          | Standard                        |
| Redundancy           | Locally-redundant storage (LRS) |

Keep all other settings as default.

Click:

```text
Review + Create
```

<img width="1131" height="547" alt="blob_review" src="https://github.com/user-attachments/assets/084d9986-d144-4136-9edf-e61907ee94f5" />


After validation succeeds, click:

```text
Create
```

<img width="1721" height="883" alt="blob" src="https://github.com/user-attachments/assets/5a6d62e7-a0f3-4a05-8ea5-179e1d0fb509" />


## **4. Open the Storage Account**

After deployment:

1. Go to:

```text
Storage Accounts
```

2. Open:

```text
xfusionst30191
```

<img width="1721" height="883" alt="blob" src="https://github.com/user-attachments/assets/f2a66303-a542-4f55-a5ce-405158aeae31" />


## **5. Create the Blob Container**

From the left menu:

```text
Data Storage → Containers
```

Click:

```text
+ Container
```



## **6. Configure the Container**

Enter:

| Setting             | Value                           |
| ------------------- | ------------------------------- |
| Name                | `xfusion-blob-12846`            |
| Public Access Level | `Private (no anonymous access)` |

Click:

```text
Create
```

<img width="1761" height="761" alt="container" src="https://github.com/user-attachments/assets/d14f8f12-96bf-4fa5-b8a8-c8e20ba7f1c5" />


## **7. Verify the Container**

Under:

```text
Containers
```

verify that:

| Property       | Expected Value       |
| -------------- | -------------------- |
| Container Name | `xfusion-blob-12846` |
| Access Level   | `Private`            |

<img width="1067" height="305" alt="containerlist" src="https://github.com/user-attachments/assets/b08fe588-df82-4507-aa48-a33f41e26048" />


# Storage Configuration Summary

| Resource        | Name                 |
| --------------- | -------------------- |
| Storage Account | `xfusionst30191`     |
| Blob Container  | `xfusion-blob-12846` |
| Access Level    | `Private`            |



# Challenges

### **1. Storage Account Name Already Exists**

Azure storage account names must be globally unique.

For this task, use exactly:

```text
xfusionst30191
```

If the lab provides this name, it will be available within the lab environment.



### **2. Wrong Container Access Level**

Make sure the container is created as:

```text
Private (no anonymous access)
```

and **not**:

```text
Blob
```

or

```text
Container**
```

public access.



### **3. Typing Mistakes**

Verify the names exactly match:

```text
xfusionst30191
```

and

```text
xfusion-blob-12846
```



### **4. Resource Group Selection**

Use the existing resource group provided by the lab environment.



## Fun Message

*"Your Azure storage vault is ready, and the blob container is locked down for private data storage ☁️📦🔒"*
