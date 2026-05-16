# Instruction

The Nautilus DevOps team is in the process of migrating some of their workloads to Azure. One of the tasks involves creating a new Virtual Machine (VM) using the Azure CLI. The team does not have access to the Azure portal but can manage Azure resources via the `azure-client` host (the landing host for this lab).

Requirements:

1. Create a new Azure Virtual Machine named `xfusion-vm` using the Azure CLI.
2. Use the `Ubuntu2204` image.
3. Set the VM size to `Standard_B2s`.
4. Ensure the admin username is `azureuser`.
5. Generate SSH keys for secure access.
6. Use `Standard_LRS` storage account type.
7. The disk size must be `30GB`.
8. Ensure the VM `xfusion-vm` is in the running state after creation.

> **Note:** Perform all operations from the `azure-client` host using Azure CLI commands only.



# Solution

## **Steps to Complete the Task**

### **1. Login to the Azure Client Host**

Access the `azure-client` host provided in the lab environment.



### **2. Login to Azure**

Authenticate using Azure CLI:

```bash id="k3v8mp"
az login
```



### **3. Check Available Resource Groups**

List the resource groups:

```bash id="w7q2nt"
az group list --output table
```

Copy the existing resource group name.



### **4. Create the Azure Virtual Machine**

Run the following command:

```bash id="u9m4xe"
az vm create \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name xfusion-vm \
  --image Ubuntu2204 \
  --size Standard_B2s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --storage-sku Standard_LRS \
  --os-disk-size-gb 30
```

Replace:

```text id="n5p1wr"
<RESOURCE_GROUP_NAME>
```

with the existing resource group name.

<img width="1016" height="461" alt="create" src="https://github.com/user-attachments/assets/d762161c-8e82-4f14-860a-f66450dd0936" />

<img width="462" height="82" alt="start" src="https://github.com/user-attachments/assets/43932675-1170-4850-8f93-942051fe0316" />

## **5. Verify the VM Status**

Check whether the VM is created and running:

```bash id="r2x7lv"
az vm list -d --output table
```

Expected output should show:

```text id="c8m3qa"
xfusion-vm
```

with power state:

```text id="f4v9ts"
VM running
```



## **6. Check VM Details**

You can inspect the VM configuration using:

```bash id="b6q1yk"
az vm show \
  --resource-group <RESOURCE_GROUP_NAME> \
  --name xfusion-vm \
  --show-details
```



## Azure Resources Used

| Resource     | Purpose                   |
| ------------ | ------------------------- |
| Azure VM     | Compute instance          |
| Managed Disk | OS storage                |
| SSH Keys     | Secure authentication     |
| Standard_LRS | Standard HDD storage type |



## VM Configuration Summary

| Setting        | Value          |
| -------------- | -------------- |
| VM Name        | `xfusion-vm`   |
| Image          | `Ubuntu2204`   |
| VM Size        | `Standard_B2s` |
| Admin Username | `azureuser`    |
| Storage Type   | `Standard_LRS` |
| Disk Size      | `30 GB`        |
| Authentication | SSH Key Based  |



## Challenges

### **1. Incorrect Image Name**

Ensure the image name is exactly:

```bash id="m7t2wc"
Ubuntu2204
```



### **2. Wrong VM Size**

Use:

```bash id="d3x8qp"
Standard_B2s
```

Other sizes may fail validation.



### **3. Missing SSH Keys**

Always include:

```bash id="p5k9zr"
--generate-ssh-keys
```

Otherwise, secure SSH access may not work properly.



### **4. Incorrect Disk Size**

Ensure the OS disk size is set using:

```bash id="g8v4ml"
--os-disk-size-gb 30
```



### **5. Resource Group Errors**

If VM creation fails, verify the resource group exists:

```bash id="y1n6xt"
az group list --output table
```



## Fun Message

*"The xFusion VM is now powered up and cruising through Azure skies ☁️🚀"*
