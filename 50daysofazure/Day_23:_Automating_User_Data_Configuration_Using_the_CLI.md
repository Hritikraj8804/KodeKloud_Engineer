# Instruction

The Nautilus DevOps Team is working on setting up a new virtual machine (VM) to host a web server for a critical application. The team lead has requested you to create an Azure VM that will serve as a web server using Nginx. This VM will be part of the initial infrastructure setup for the Nautilus project. Ensuring that the server is correctly configured and accessible from the internet is crucial for the upcoming deployment phase.

As a member of the Nautilus DevOps Team, your task is to create a VM using Azure CLI with the following specifications:

Instance Name: The VM must be named devops-vm.

Image: Use any available Ubuntu image to create this VM.

Custom Script Extension/User Data: Configure the VM to run a custom script during its launch. This script should:

Install the Nginx package.
Start the Nginx service.
Network Security Group (NSG): Ensure that the VM allows HTTP traffic on port 80 from the internet.

Instructions:

Use Azure CLI commands to set up the VM in the specified configuration.
Ensure the VM is accessible from the internet on port 80.
The Nginx service should be running after setup.


Use the Azure CLI commands to complete the task.


Notes:

Create the resources only in the East US region.
You may use the default resource group or create a new one if needed.

# Solution

## 1. Set Resource Group

```bash
RG_NAME="kml_rg_main-f278423855774c95"
```

<img width="467" height="97" alt="group" src="https://github.com/user-attachments/assets/bf6777df-c690-4fb3-9c2d-3720baa17c05" />


## 2. Create Cloud-Init File

```bash
cat > cloud-init.txt <<EOF
#cloud-config
package_update: true
packages:
  - nginx

runcmd:
  - systemctl enable nginx
  - systemctl start nginx
EOF
```

<img width="310" height="191" alt="init" src="https://github.com/user-attachments/assets/6e43c7e3-0053-476d-ba7b-cae76f3ebb9b" />


## 3. Create the VM

Use a small VM size to stay within lab quotas:

```bash
az vm create \
  --resource-group $RG_NAME \
  --name devops-vm \
  --location eastus \
  --image Ubuntu2204 \
  --size Standard_B1s \
  --admin-username azureuser \
  --generate-ssh-keys \
  --custom-data cloud-init.txt
```

Wait until provisioning completes.

<img width="1031" height="440" alt="vmcreate" src="https://github.com/user-attachments/assets/4c9a604e-2772-47f1-920c-3d0f29316e54" />


## 4. Open HTTP Port 80

```bash
az vm open-port \
  --resource-group $RG_NAME \
  --name devops-vm \
  --port 80
```

<img width="635" height="276" alt="allowport" src="https://github.com/user-attachments/assets/d6547670-6a17-4da3-9f31-adffa63e4ee8" />


## 5. Verify VM is Running

```bash
az vm list -d \
  --query "[?name=='devops-vm'].[name,powerState]" \
  -o table
```

Expected:

```text
devops-vm    VM running
```



## 6. Get Public IP

```bash
az vm show \
  --resource-group $RG_NAME \
  --name devops-vm \
  -d \
  --query publicIps \
  -o tsv
```

Example:

```text
20.x.x.x
```

<img width="877" height="665" alt="ip" src="https://github.com/user-attachments/assets/eca95a7b-c42f-4c13-8af0-577d6e138713" />


## 7. Verify Nginx

Wait 1–2 minutes for cloud-init to finish, then:

```bash
curl http://$(az vm show \
  --resource-group $RG_NAME \
  --name devops-vm \
  -d \
  --query publicIps \
  -o tsv)
```

Expected output contains:

```html
Welcome to nginx!
```

<img width="1307" height="421" alt="verify" src="https://github.com/user-attachments/assets/5765cccc-3956-4ece-b5cf-5c325796cd93" />


## If the validator still fails

SSH into the VM:

```bash
ssh azureuser@<PUBLIC_IP>
```

Check:

```bash
sudo systemctl status nginx
```

If needed:

```bash
sudo systemctl restart nginx
sudo systemctl enable nginx
```


