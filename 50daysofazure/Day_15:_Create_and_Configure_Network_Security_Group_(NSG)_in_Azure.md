# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the Azure cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.

For this task, create a **Network Security Group (NSG)** with the following requirements:

| Setting  | Value         |
| -------- | ------------- |
| NSG Name | `xfusion-nsg` |

### Inbound Security Rules

| Rule Name    | Protocol | Port | Source      |
| ------------ | -------- | ---- | ----------- |
| `Allow-HTTP` | TCP      | 80   | `0.0.0.0/0` |
| `Allow-SSH`  | TCP      | 22   | `0.0.0.0/0` |

> **Note:** Use the Azure Portal (UI/Console) to complete this task.



# Solution

## **1. Login to Azure Portal**

Open:

```text
https://portal.azure.com
```

Login using your Azure credentials.



## **2. Open Network Security Groups Service**

In the Azure Portal search bar, search for:

```text
Network Security Groups
```

Click:

```text
Network Security Groups
```

from the services list.



## **3. Create a New Network Security Group**

Click:

```text
+ Create
```



## **4. Configure the NSG**

Fill in the following details:

| Setting        | Value                          |
| -------------- | ------------------------------ |
| Resource Group | Select existing resource group |
| Name           | `xfusion-nsg`                  |
| Region         | Select the lab/default region  |

Then click:

```text
Review + Create
```

After validation succeeds, click:

```text
Create
```

<img width="648" height="672" alt="review" src="https://github.com/user-attachments/assets/741e19fb-3c7b-4cfe-bad8-ed615792d8af" />


## **5. Open the Newly Created NSG**

After deployment:

1. Go to:

```text
Network Security Groups
```

2. Open:

```text
xfusion-nsg
```

<img width="1902" height="778" alt="ngsbefore" src="https://github.com/user-attachments/assets/487dc33b-281d-457f-a44e-cb2a6b3c695d" />


# Create Rule 1: Allow HTTP

## **6. Open Inbound Security Rules**

From the left menu:

```text
Settings → Inbound security rules
```

Click:

```text
+ Add
```



## **7. Configure HTTP Rule**

Fill in:

| Setting                         | Value        |
| ------------------------------- | ------------ |
| Source                          | IP Addresses |
| Source IP Addresses/CIDR Ranges | `0.0.0.0/0`  |
| Source Port Ranges              | `*`          |
| Destination                     | Any          |
| Service                         | HTTP         |
| Action                          | Allow        |
| Priority                        | `1000`       |
| Name                            | `Allow-HTTP` |

Click:

```text
Add
```

<img width="476" height="851" alt="http" src="https://github.com/user-attachments/assets/de20878d-b502-4347-a633-fc04b03c2d69" />


# Create Rule 2: Allow SSH

## **8. Add Another Inbound Rule**

Click:

```text
+ Add
```



## **9. Configure SSH Rule**

Fill in:

| Setting                         | Value        |
| ------------------------------- | ------------ |
| Source                          | IP Addresses |
| Source IP Addresses/CIDR Ranges | `0.0.0.0/0`  |
| Source Port Ranges              | `*`          |
| Destination                     | Any          |
| Service                         | SSH          |
| Action                          | Allow        |
| Priority                        | `1010`       |
| Name                            | `Allow-SSH`  |

Click:

```text
Add
```

<img width="470" height="848" alt="ssh" src="https://github.com/user-attachments/assets/86a9a689-5a82-4aac-bbc2-43b458bcd830" />


## **10. Verify the Rules**

Under:

```text
Inbound security rules
```

you should see:

| Rule Name    | Port | Source      |
| ------------ | ---- | ----------- |
| `Allow-HTTP` | 80   | `0.0.0.0/0` |
| `Allow-SSH`  | 22   | `0.0.0.0/0` |


<img width="1375" height="291" alt="cli" src="https://github.com/user-attachments/assets/41f4a9d4-4523-42e4-93c2-9221b62a6f49" />


# NSG Configuration Summary

| Setting     | Value         |
| ----------- | ------------- |
| NSG Name    | `xfusion-nsg` |
| Rule 1      | `Allow-HTTP`  |
| Port        | `80`          |
| Rule 2      | `Allow-SSH`   |
| Port        | `22`          |
| Source CIDR | `0.0.0.0/0`   |

<img width="1902" height="378" alt="nsgbefre" src="https://github.com/user-attachments/assets/ac24c7e5-65dc-45a6-990d-bc8ab0beb1eb" />


# Challenges

### **1. Incorrect Rule Names**

Make sure the rule names are exactly:

```text
Allow-HTTP
```

and

```text
Allow-SSH
```



### **2. Wrong Source CIDR**

Verify the source is:

```text
0.0.0.0/0
```

for both rules.



### **3. Priority Conflicts**

Each NSG rule must have a unique priority.

Example:

```text
Allow-HTTP → 1000
Allow-SSH → 1010
```



### **4. Wrong Service Selection**

Choose:

```text
HTTP
```

for port 80 and

```text
SSH
```

for port 22.



## Fun Message

*"Your Azure firewall gate is now open for web traffic and secure SSH access ☁️🛡️🚀"*
