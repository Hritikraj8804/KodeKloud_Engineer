# Instruction

The Nautilus DevOps Team is setting up a new Azure Virtual Machine to host an Nginx web server.

### Requirements

| Setting        | Value                              |
| ----------- | ---------------------------------- |
| VM Name        | `nautilus-vm`                      |
| Image          | Any Ubuntu Image                   |
| Web Server     | Nginx                              |
| Startup Script | Install and start Nginx            |
| NSG Rule       | Allow HTTP (Port 80) from Internet |

> **Note:** Perform all operations using Azure CLI from the `azure-client` host.



# Solution

## **1. Login to Azure**

```bash
az login
```



## **2. Find the Resource Group**

```bash
az group list --query "[].name" -o table
```

Save the resource group name:

```bash
RG_NAME=<RESOURCE_GROUP>
```

<img width="577" height="50" alt="rg" src="https://github.com/user-attachments/assets/a2fdc789-4140-409c-9f7c-aabe6a5b4403" />

## **3. Create a Cloud-Init Script**

Create a file:

```bash
vi cloud-init.txt
```

Add the following content:

```yaml
#cloud-config
package_update: true
packages:
  - nginx

runcmd:
  - systemctl enable nginx
  - systemctl start nginx
```

Save the file.



## **4. Create the Virtual Machine**

```bash
az vm create \
  --resource-group $RG_NAME \
  --name nautilus-vm \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --custom-data cloud-init.txt
```

You may use any available Ubuntu image:

```bash
Ubuntu2204
```

or

```bash
Ubuntu2404
```

depending on availability.

<img width="1002" height="522" alt="vmcreate" src="https://github.com/user-attachments/assets/ae72aadd-7526-4c93-98ee-aca59b67bcaf" />

## **5. Allow HTTP Traffic (Port 80)**

Create an NSG rule for HTTP:

```bash
az vm open-port \
  --resource-group $RG_NAME \
  --name nautilus-vm \
  --port 80
```

<img width="838" height="303" alt="post" src="https://github.com/user-attachments/assets/61cf26d3-a428-4ff3-82c5-03be9214472c" />

## **6. Verify VM is Running**

```bash
az vm list -d \
  --query "[?name=='nautilus-vm'].[name,powerState]" \
  -o table
```

Expected:

```text
nautilus-vm   VM running
```



## **7. Get the Public IP Address**

```bash
az vm show \
  --resource-group $RG_NAME \
  --name nautilus-vm \
  -d \
  --query publicIps \
  -o tsv
```

Example:

```text
20.xx.xx.xx
```

<img width="693" height="160" alt="port80" src="https://github.com/user-attachments/assets/20c37fb6-fa9c-488c-9cd9-b1eb7ac272dc" />

## **8. Verify Nginx via Browser or Curl**

Using the public IP:

```bash
curl http://<PUBLIC_IP>
```

Example:

```bash
curl http://20.xx.xx.xx
```

Expected output contains:

```html
Welcome to nginx!
```


<img width="1417" height="437" alt="verify" src="https://github.com/user-attachments/assets/8ea7f140-34f2-471d-8825-3649f9fbb884" />


## **9. Optional SSH Verification**

```bash
ssh azureuser@<PUBLIC_IP>
```

Check Nginx:

```bash
systemctl status nginx
```

Expected:

```text
active (running)
```


## Fun Message

*"Your Nautilus web server is now sailing on Azure with Nginx serving traffic from port 80 ☁️🌐🚀"*
