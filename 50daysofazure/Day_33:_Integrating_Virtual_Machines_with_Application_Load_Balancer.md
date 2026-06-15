# Instruction

The Nautilus DevOps team needs to set up an Azure Load Balancer in front of an existing VM running Nginx.

### Requirements

| Resource           | Name                  |
| ------------------ | --------------------- |
| Load Balancer      | `devops-lb`           |
| Frontend IP Config | `devops-lb-ip`        |
| Public IP          | `devops-lb-ip`        |
| Backend Pool       | `devops-backend-pool` |
| Health Probe       | `devops-health-probe` |
| LB Rule            | `devops-lb-rule`      |
| Region             | `eastus`              |

Additional requirements:

* Add the VM running Nginx to the backend pool.
* Health probe on port **80**.
* Load balancing rule from **80 → 80**.
* Add an NSG rule to allow HTTP (TCP/80).


# Solution (Azure CLI)

## 1. Get Resource Group and VM Details

Find the resource group:

```bash
az group list -o table
```

<img width="1182" height="122" alt="resorce table" src="https://github.com/user-attachments/assets/ef76e68e-19eb-41bb-bf39-c2a9068bffdb" />

Example:

```bash
RG_NAME="kml_rg_main-xxxx"
```

Find the VM running Nginx:

```bash
az vm list -d -o table
```

Assume the VM name is:

```bash
VM_NAME="<NGINX_VM_NAME>"
```


## 2. Create Static Public IP

```bash
az network public-ip create \
  --resource-group $RG_NAME \
  --name devops-lb-ip \
  --sku Standard \
  --allocation-method Static \
  --location eastus
```

<img width="622" height="742" alt="create ip" src="https://github.com/user-attachments/assets/2d51c54c-90d8-494f-8571-7250700b85a5" />

## 3. Create Load Balancer

```bash
az network lb create \
  --resource-group $RG_NAME \
  --name devops-lb \
  --sku Standard \
  --location eastus \
  --frontend-ip-name devops-lb-ip \
  --public-ip-address devops-lb-ip \
  --backend-pool-name devops-backend-pool
```

<img width="627" height="282" alt="load balancer create" src="https://github.com/user-attachments/assets/d162eca4-1b56-4024-a1bc-94b72e7758a2" />

## 4. Create Health Probe

```bash
az network lb probe create \
  --resource-group $RG_NAME \
  --lb-name devops-lb \
  --name devops-health-probe \
  --protocol tcp \
  --port 80
```

<img width="712" height="512" alt="lb probe" src="https://github.com/user-attachments/assets/aa32bf84-e9a6-47c9-8da6-197a6785a21a" />

## 5. Create Load Balancer Rule

```bash
az network lb rule create \
  --resource-group $RG_NAME \
  --lb-name devops-lb \
  --name devops-lb-rule \
  --protocol Tcp \
  --frontend-port 80 \
  --backend-port 80 \
  --frontend-ip-name devops-lb-ip \
  --backend-pool-name devops-backend-pool \
  --probe-name devops-health-probe
```


## 6. Add VM NIC to Backend Pool

Get NIC name:

```bash
NIC_NAME=$(az vm show \
  -g $RG_NAME \
  -n $VM_NAME \
  --query "networkProfile.networkInterfaces[0].id" \
  -o tsv | awk -F/ '{print $NF}')
```

Get NIC IP configuration name:

```bash
az network nic show \
  -g $RG_NAME \
  -n $NIC_NAME \
  --query "ipConfigurations[].name" \
  -o tsv
```

Usually returns:

```text
ipconfig1
```

Add NIC to backend pool:

```bash
az network nic ip-config address-pool add \
  --resource-group $RG_NAME \
  --nic-name $NIC_NAME \
  --ip-config-name ipconfig1 \
  --lb-name devops-lb \
  --address-pool devops-backend-pool
```

<img width="737" height="501" alt="network nip" src="https://github.com/user-attachments/assets/e5ff4099-3689-47e5-82b0-8d7cab291fff" />

## 7. Allow HTTP in NSG

Get NSG name attached to VM NIC:

```bash
az network nic show \
  -g $RG_NAME \
  -n $NIC_NAME \
  --query "networkSecurityGroup.id" \
  -o tsv
```

Create HTTP rule:

```bash
az network nsg rule create \
  --resource-group $RG_NAME \
  --nsg-name <NSG_NAME> \
  --name Allow-HTTP \
  --priority 1000 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --source-address-prefixes '*' \
  --source-port-ranges '*' \
  --destination-port-ranges 80
```

Replace:

```bash
<NSG_NAME>
```

with the actual NSG name.

<img width="606" height="342" alt="netowk rule" src="https://github.com/user-attachments/assets/25ce5b43-d6bf-470f-b7ec-52b48cb734a9" />

## 8. Verify Nginx

SSH into the VM:

```bash
ssh azureuser@<VM_PUBLIC_IP>
```

Check:

```bash
sudo systemctl status nginx
```

If not running:

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl restart nginx
```


## 9. Get Load Balancer Public IP

```bash
az network public-ip show \
  --resource-group $RG_NAME \
  --name devops-lb-ip \
  --query ipAddress \
  -o tsv
```

Example:

```text
20.x.x.x
```

<img width="1241" height="347" alt="public ip" src="https://github.com/user-attachments/assets/1ae64855-b5e0-4865-a161-1a2671387d49" />

## 10. Test the Load Balancer

```bash
curl http://<LB_PUBLIC_IP>
```

Expected:

```html
Welcome to nginx!
```

<img width="1446" height="557" alt="verify nginx" src="https://github.com/user-attachments/assets/761e9e51-43d1-4357-9ab0-66ce9af1cfeb" />
