# Instruction

The Nautilus DevOps team is working on setting up secure SSH access for their virtual machines in Azure.

### Requirements

* VM Name: `xfusion-vm`
* Region: `centralus`
* Default SSH User: `azureuser`
* Public key source:

```bash
/root/.ssh/id_rsa.pub
```

(from the `azure-client` host)

### Tasks

1. Copy the root user's public key from the Azure client host to the VM.
2. Add the key to the root user's `authorized_keys` file on `xfusion-vm`.
3. Set proper permissions on the `.ssh` directory and `authorized_keys` file.
4. Verify password-less SSH access as `root` from the Azure client host.

> **Note:** This task requires a combination of Azure Portal (UI) and terminal access.



# Solution

## Step 1: Get the VM Public IP from Azure Portal

### Open the VM

1. Login to Azure Portal:

```text
https://portal.azure.com
```

2. Search:

```text
Virtual Machines
```

3. Open:

```text
xfusion-vm
```

4. Copy the:

```text
Public IP Address
```

from the Overview page.

Example:

```text
20.xx.xx.xx
```

<img width="1887" height="703" alt="vmlist" src="https://github.com/user-attachments/assets/90de4851-c541-400d-99ce-c489fe43f954" />


## Step 2: Verify SSH Port is Open

Inside the VM page:

### Networking

Verify there is an inbound rule:

| Protocol | Port |
| -------- | ---- |
| TCP      | 22   |

If not:

1. Click **Add inbound port rule**
2. Select:

```text
SSH (22)
```

3. Save the rule.

<img width="787" height="142" alt="inbound" src="https://github.com/user-attachments/assets/28cf22fe-b42b-45e7-8748-43d45e3dd96a" />


## Step 3: Read the Root Public Key on Azure Client Host

Login to the Azure client host and display the key:

```bash
cat /root/.ssh/id_rsa.pub
```

Copy the complete output.

Example:

```text
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQ...
```



## Step 4: Connect to the VM as azureuser

From the Azure client host:

```bash
ssh azureuser@<PUBLIC_IP>
```

Example:

```bash
ssh azureuser@20.xx.xx.xx
```



## Step 5: Become Root on the VM

After logging in:

```bash
sudo su -
```



## Step 6: Create Root SSH Directory

```bash
mkdir -p /root/.ssh
chmod 700 /root/.ssh
```



## Step 7: Add the Public Key

Open:

```bash
vi /root/.ssh/authorized_keys
```

Paste the public key copied from:

```bash
/root/.ssh/id_rsa.pub
```

Save and exit.

<img width="1002" height="412" alt="authorikey" src="https://github.com/user-attachments/assets/1b7e4b48-445e-4c39-bab5-316620cb91db" />


## Step 8: Set Correct Permissions

```bash
chmod 600 /root/.ssh/authorized_keys
chmod 700 /root/.ssh
```

Verify:

```bash
ls -ld /root/.ssh
ls -l /root/.ssh/authorized_keys
```

Expected:

```text
drwx------
-rw-------
```



## Step 9: Enable Root SSH Login (If Needed)

Check:

```bash
sudo vi /etc/ssh/sshd_config
```

Ensure:

```text
PermitRootLogin yes
PubkeyAuthentication yes
```

<img width="340" height="195" alt="permission" src="https://github.com/user-attachments/assets/77ccad06-793d-4a08-894c-4abadc0a34c1" />


If changes are made, restart SSH:

Ubuntu:

```bash
systemctl restart ssh
```

or

```bash
systemctl restart sshd
```



## Step 10: Verify Password-less Root Login

Exit the VM:

```bash
exit
exit
```

From the Azure client host:

```bash
ssh root@<PUBLIC_IP>
```

Example:

```bash
ssh root@20.xx.xx.xx
```

You should log in directly without being prompted for a password.

<img width="627" height="322" alt="rootssh1" src="https://github.com/user-attachments/assets/f77f115c-8198-4e81-b480-0e9d6280db85" />
<img width="677" height="212" alt="rootssh2" src="https://github.com/user-attachments/assets/dc1e2b43-59ac-4f68-8048-66311afd33e1" />


# Verification Checklist

✅ VM `xfusion-vm` is running

✅ Port 22 is open

✅ Root public key copied from:

```bash
/root/.ssh/id_rsa.pub
```

✅ Key added to:

```bash
/root/.ssh/authorized_keys
```

✅ Correct permissions applied

```bash
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
```

✅ Password-less SSH as root works



# Common Issues

### Permission Denied (publickey)

Verify:

```bash
chmod 700 /root/.ssh
chmod 600 /root/.ssh/authorized_keys
```



### Root Login Disabled

Check:

```text
PermitRootLogin yes
```

inside:

```bash
/etc/ssh/sshd_config
```



### SSH Timeout

Verify NSG inbound rule:

```text
TCP 22
```

is allowed.



## Fun Message

*"The Azure VM now trusts the Nautilus root key and welcomes password-less SSH voyages 🚀🔐☁️"*
