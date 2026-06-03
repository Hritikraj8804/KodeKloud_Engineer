# Instruction

The Nautilus DevOps Team has received a request to create a new Azure Virtual Machine with a **Static Public IP**.

### Requirements

| Resource        | Name             |
| --------------- | ---------------- |
| Virtual Machine | `datacenter-vm`  |
| Public IP       | `datacenter-pip` |
| VM Size         | `Standard_B1s`   |
| OS Image        | Any Ubuntu Image |
| Authentication  | SSH Key          |
| Public IP Type  | Static           |

Tasks:

1. Generate an SSH key pair on the `azure-client` host.
2. Create a Static Public IP named `datacenter-pip`.
3. Create a VM named `datacenter-vm` using any Ubuntu image and size `Standard_B1s`.
4. Associate the generated SSH public key with the VM.
5. Associate the Static Public IP with the VM.
6. Verify SSH connectivity.

> **Note:** Perform the task using Azure CLI from the `azure-client` host.



# Solution

## **1. Generate SSH Key Pair**

On the `azure-client` host:

```bash
ssh-keygen -t rsa -f ~/.ssh/datacenter_key -N ""
```

This creates:

```bash
~/.ssh/datacenter_key
~/.ssh/datacenter_key.pub
```

<img width="1025" height="458" alt="keygen" src="https://github.com/user-attachments/assets/c70fcb0a-13de-4b71-96d4-a61d76c34abf" />


## **2. Find the Resource Group**

```bash
az group list --output table
```

Copy the existing resource group name.

Example:

```bash
RG_NAME=<RESOURCE_GROUP>
```


## **3. Create a Static Public IP**

```bash
az network public-ip create \
  --resource-group $RG_NAME \
  --name datacenter-pip \
  --sku Standard \
  --allocation-method Static
```

<img width="681" height="755" alt="create ip" src="https://github.com/user-attachments/assets/d99594da-2660-4538-a190-5f771f5c29a7" />


## **4. Create the Virtual Machine**

```bash
az vm create \
  --resource-group $RG_NAME \
  --name datacenter-vm \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --ssh-key-values ~/.ssh/datacenter_key.pub \
  --public-ip-address datacenter-pip
```

You may also use another available Ubuntu image such as:

```bash
Ubuntu2404
```

if supported in the lab.

<img width="1036" height="365" alt="createvm" src="https://github.com/user-attachments/assets/9685deb7-ee1e-4c87-8832-e36c975518f4" />


## **5. Open SSH Port (22)**

```bash
az vm open-port \
  --resource-group $RG_NAME \
  --name datacenter-vm \
  --port 22
```


## **6. Verify the Public IP**

```bash
az vm show \
  --resource-group $RG_NAME \
  --name datacenter-vm \
  -d \
  --query publicIps \
  -o tsv
```

Example output:

```text
20.xx.xx.xx
```

<img width="777" height="82" alt="ip" src="https://github.com/user-attachments/assets/2658b1c2-c7f2-4370-a788-738f5e8330ae" />


## **7. Verify the Public IP is Static**

```bash
az network public-ip show \
  --resource-group $RG_NAME \
  --name datacenter-pip \
  --query publicIPAllocationMethod
```

Expected:

```text
Static
```



## **8. Test SSH Access**

```bash
ssh -i ~/.ssh/datacenter_key azureuser@<PUBLIC_IP>
```

Example:

```bash
ssh -i ~/.ssh/datacenter_key azureuser@20.xx.xx.xx
```

Expected result:

```text
Welcome to Ubuntu
```
<img width="677" height="723" alt="ssh" src="https://github.com/user-attachments/assets/e964de71-116d-40fb-a435-58387b29684d" />

## Fun Message

*"Your datacenter VM is now online with a permanent Azure address and secure SSH access ☁️🔐🚀"*
