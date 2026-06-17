# Instruction

The Nautilus DevOps team needs to demonstrate **Azure VNet Peering** between a public VNet and a private VNet.

### Existing Resources

| Resource       | Name                  |
| -------------- | --------------------- |
| Public VM      | `xfusion-pub-vm`      |
| Private VM     | `xfusion-priv-vm`     |
| Private VNet   | `xfusion-priv-vnet`   |
| Private Subnet | `xfusion-priv-subnet` |
| Region         | `East US`             |

### Tasks

1. Create a VNet peering named:

```text
xfusion-pub-to-priv-peering
```

2. Peer the Public VNet with the Private VNet.
3. Verify connectivity by SSHing into `xfusion-pub-vm` and pinging `xfusion-priv-vm`.

> **Note:** Perform all tasks using Azure CLI.

# Solution

## **1. Find the Resource Group**

```bash
az group list -o table
```

Example:

```bash
RG_NAME="kml_rg_main-xxxxxxxx"
```

<img width="952" height="357" alt="resource" src="https://github.com/user-attachments/assets/767d050f-e844-4b1e-81c4-27ecd68f42c5" />

## **2. Identify the Public VNet Name**

The task only provides the private VNet name.

List VNets:

```bash
az network vnet list \
  --resource-group $RG_NAME \
  -o table
```

Example output:

```text
Name
-------------------
xfusion-pub-vnet
xfusion-priv-vnet
```

Save:

```bash
PUB_VNET="xfusion-pub-vnet"
PRIV_VNET="xfusion-priv-vnet"
```

<img width="1037" height="191" alt="nic list" src="https://github.com/user-attachments/assets/5d697f6d-4ce6-4a5e-84f0-f9f326cc9a68" />

<img width="997" height="97" alt="vnet list" src="https://github.com/user-attachments/assets/534b9985-0ee3-4457-afff-fe241239b32b" />

## **3. Create Peering from Public → Private**

```bash
az network vnet peering create \
  --resource-group $RG_NAME \
  --name xfusion-pub-to-priv-peering \
  --vnet-name $PUB_VNET \
  --remote-vnet $PRIV_VNET \
  --allow-vnet-access
```

Expected:

```text
"peeringState": "Initiated"
```

## **4. Create Reverse Peering (Required)**

Azure VNet peering is typically configured in both directions.

Create a reverse peering:

```bash
az network vnet peering create \
  --resource-group $RG_NAME \
  --name xfusion-priv-to-pub-peering \
  --vnet-name $PRIV_VNET \
  --remote-vnet $PUB_VNET \
  --allow-vnet-access
```

## **5. Verify Peering Status**

```bash
az network vnet peering list \
  --resource-group $RG_NAME \
  --vnet-name $PUB_VNET \
  -o table
```

Expected:

```text
Name                          PeeringState
----------------------------  ------------
xfusion-pub-to-priv-peering   Connected
```

<img width="492" height="296" alt="create peering pub-to-pri" src="https://github.com/user-attachments/assets/a3d722de-ce34-46b2-b4fa-6fb2271ffe3c" />

Verify the reverse side:

```bash
az network vnet peering list \
  --resource-group $RG_NAME \
  --vnet-name $PRIV_VNET \
  -o table
```

Expected:

```text
xfusion-priv-to-pub-peering   Connected
```

<img width="511" height="267" alt="create peering pri-to-pub" src="https://github.com/user-attachments/assets/05117413-b47f-43c7-bbe7-499e26c3a619" />

# Test Connectivity

## **6. Get the Public VM IP Address**

```bash
az vm show \
  --resource-group $RG_NAME \
  --name xfusion-pub-vm \
  -d \
  --query publicIps \
  -o tsv
```

Example:

```text
20.xx.xx.xx
```

<img width="542" height="157" alt="pub vm ip" src="https://github.com/user-attachments/assets/61cc15ee-db47-46b5-8818-2e65629cea4a" />

## **7. Get the Private VM IP Address**

```bash
az vm list-ip-addresses \
  --resource-group $RG_NAME \
  --name xfusion-priv-vm \
  --query "[0].virtualMachine.network.privateIpAddresses[0]" \
  -o tsv
```

Example:

```text
10.0.1.4
```

<img width="446" height="131" alt="priv vm ip" src="https://github.com/user-attachments/assets/f0c54151-23a7-4f40-b9be-d043bc4edff7" />

## **8. SSH into the Public VM**

```bash
ssh azureuser@<PUBLIC_VM_IP>
```

Example:

```bash
ssh azureuser@20.xx.xx.xx
```

<img width="707" height="331" alt="ssh" src="https://github.com/user-attachments/assets/be92a67e-9bc8-4558-9272-9c4effc0947b" />

## **9. Ping the Private VM**

Inside the public VM:

```bash
ping <PRIVATE_VM_IP>
```

Example:

```bash
ping 10.0.1.4
```

Expected:

```text
64 bytes from 10.0.1.4
```

<img width="487" height="447" alt="ping" src="https://github.com/user-attachments/assets/d13a97c3-9f21-480c-ada4-250fedad8681" />

showing successful communication through the VNet peering.

## Fun Message

*"The public and private Azure networks are now shaking hands through VNet Peering, allowing secure cross-network communication 🚀☁️🔗"*
