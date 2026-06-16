# Instruction

The Nautilus DevOps team has encountered a connectivity issue on the Azure VM `nautilus-vm`. Due to this issue, package installation commands such as `apt update` and `apt install` are failing.

Your task is to:

* Investigate the root cause of the connectivity problem.
* Implement a fix to restore outbound internet access.
* Verify that package installation works correctly after the fix.

> **Note:** The SSH private key is already available on the `azure-client` host at:
>
> ```bash
> /root/.ssh/id_rsa
> ```
>
> All resources are located in the **East US** region.

# Solution

## **1. Inspect the Network Security Group (NSG)**

List the NSG rules associated with the VM:

```bash
az network nsg rule list \
  -g kml_rg_main-cacccfaeaa524e31 \
  --nsg-name nautilus-nsg \
  -o table
```

Output:

```text
Name                Priority  Access  Direction
------------------  --------  ------  ---------
Allow-SSH           100       Allow   Inbound
Allow-HTTP          110       Allow   Inbound
Block-All-Outbound  200       Deny    Outbound
```

## **2. Identify the Root Cause**

The rule:

```text
Block-All-Outbound
```

<img width="992" height="217" alt="nsg rule before" src="https://github.com/user-attachments/assets/a603a7d8-19c0-4a0f-8346-8c04625eba3d" />

is denying **all outbound traffic** from the VM.

This prevents access to:

* Ubuntu package repositories
* DNS services
* External internet resources

As a result, commands such as:

```bash
sudo apt update
sudo apt install nginx
```

<img width="861" height="461" alt="apt update" src="https://github.com/user-attachments/assets/b0d40ece-ca24-4336-a695-382cad69d3b2" />

fail due to lack of connectivity.

## **3. Verify the Issue from the VM**

Get the VM public IP:

```bash
az vm list-ip-addresses \
  --name nautilus-vm \
  --resource-group kml_rg_main-cacccfaeaa524e31 \
  --output table
```

<img width="862" height="110" alt="vm list" src="https://github.com/user-attachments/assets/568fe97d-2af4-481c-aa06-d4452963004f" />

SSH into the VM:

```bash
ssh -i /root/.ssh/id_rsa azureuser@<PUBLIC_IP>
```

<img width="725" height="241" alt="vm ssh" src="https://github.com/user-attachments/assets/ca56953c-2cbd-4534-8201-504c8547ee29" />

Test internet connectivity:

```bash
curl https://archive.ubuntu.com
```

or

```bash
ping 8.8.8.8
```

These commands should fail while the outbound deny rule exists.

<img width="737" height="107" alt="packet loss" src="https://github.com/user-attachments/assets/a9d7de2d-dc5f-45f0-8083-4b6322efc437" />

## **4. Remove the Blocking Rule**

Delete the outbound deny rule:

```bash
az network nsg rule delete \
  -g kml_rg_main-cacccfaeaa524e31 \
  --nsg-name nautilus-nsg \
  --name Block-All-Outbound
```

<img width="302" height="82" alt="nsg rule fix" src="https://github.com/user-attachments/assets/ac15cfb6-77a0-4def-bc12-bb4033d08383" />

## **5. Verify the NSG Configuration**

List the rules again:

```bash
az network nsg rule list \
  -g kml_rg_main-cacccfaeaa524e31 \
  --nsg-name nautilus-nsg \
  -o table
```

Expected output:

```text
Allow-SSH
Allow-HTTP
```

The following rule should no longer exist:

```text
Block-All-Outbound
```

<img width="977" height="192" alt="nsg fix verify" src="https://github.com/user-attachments/assets/ba0a415f-7863-40f4-9648-50a5d44855bd" />

## **6. Verify Package Installation Works**

SSH back into the VM:

```bash
ssh -i /root/.ssh/id_rsa azureuser@<PUBLIC_IP>
```

Update package repositories:

```bash
sudo apt update
```

Expected:

```text
Reading package lists... Done
```

Install a package for verification:

```bash
sudo apt install nginx -y
```

Expected:

```text
Setting up nginx ...
```

without any connectivity errors.

## **7. Confirm Connectivity Restoration**

Verify outbound access:

```bash
curl https://archive.ubuntu.com
```

or

```bash
wget https://archive.ubuntu.com
```

The request should now succeed.

# Root Cause Summary

| Issue                                                             | Resolution                                           |
| ----------------------------------------------------------------- | ---------------------------------------------------- |
| Outbound internet access blocked by NSG rule `Block-All-Outbound` | Delete the NSG rule to restore outbound connectivity |

## Fun Message

*"The VM can once again reach the outside world, and package installations are back on course 🚀☁️🔧"*
