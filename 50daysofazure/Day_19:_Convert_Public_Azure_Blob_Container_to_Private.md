# Instruction

The Nautilus DevOps team has been using Azure Blob Storage to manage their data. One of their blob containers is currently public and needs to be converted to private.

### Requirements

Storage Account:

```text
devopsst9734
```

Blob Containers:

```text
devops-container-20434
devops-priv-17810
```

Tasks:

1. Change **devops-container-20434** from **Public** to **Private**.
2. Leave **devops-priv-17810** unchanged.
3. Ensure **devops-container-20434** has:

```text
Private (no anonymous access)
```

access level after the change.

> **Note:** Use the Azure Portal (UI/Console) to complete this task.



# Solution

## **1. Login to Azure Portal**

Open:

```text
https://portal.azure.com
```

Login using the provided Azure credentials.



## **2. Open the Storage Account**

In the Azure Portal search bar, search for:

```text
Storage Accounts
```

Open the storage account:

```text
devopsst9734
```
<img width="1167" height="687" alt="previous" src="https://github.com/user-attachments/assets/fc23366e-e259-483d-81c6-0a3a83ff1233" />



## **3. Open Blob Containers**

From the left menu:

```text
Data Storage → Containers
```

You should see:

```text
devops-container-20434
devops-priv-17810
```



## **4. Open the Public Container**

Click:

```text
devops-container-20434
```



## **5. Change Access Level**

At the top of the container page, click:

```text
Change access level
```

or

```text
Change access policy
```

(depending on the Azure Portal version).

<img width="1095" height="521" alt="precont" src="https://github.com/user-attachments/assets/947bd8cf-8bba-49bd-936f-bf1487488f79" />


## **6. Set Container to Private**

For **Public access level**, select:

```text
Private (no anonymous access)
```

Then click:

```text
Save
```

<img width="657" height="480" alt="accesslevel" src="https://github.com/user-attachments/assets/832260d0-326f-43c8-a8df-4f06ed17bdb0" />


## **7. Verify the Change**

After saving, verify the container shows:

| Property            | Value                           |
| ------------------- | ------------------------------- |
| Container Name      | `devops-container-20434`        |
| Public Access Level | `Private (no anonymous access)` |



## **8. Leave the Second Container Unchanged**

Do **not** modify:

```text
devops-priv-17810
```

It should remain private as it already meets the requirement.

<img width="1042" height="480" alt="aftercont" src="https://github.com/user-attachments/assets/ed5461bd-34c7-46a3-a011-df373d32f0ae" />


# Verification Checklist

✅ Storage Account:

```text
devopsst9734
```

✅ Container:

```text
devops-container-20434
```

changed to:

```text
Private (no anonymous access)
```

✅ Container:

```text
devops-priv-17810
```

left unchanged



# Configuration Summary

| Container                | Expected Access               |
| ------------------------ | ----------------------------- |
| `devops-container-20434` | Private (no anonymous access) |
| `devops-priv-17810`      | Private (unchanged)           |



# Common Issues

### Container Still Shows Public

After changing the access level, refresh the container page and verify:

```text
Private (no anonymous access)
```

is displayed.



### Modified Wrong Container

Only update:

```text
devops-container-20434
```

Do not edit:

```text
devops-priv-17810
```



### Save Button Not Clicked

The change is not applied until you click:

```text
Save
```



## Fun Message

*"The public storage door has been locked, and the container is now securely private 🔒☁️📦"*
