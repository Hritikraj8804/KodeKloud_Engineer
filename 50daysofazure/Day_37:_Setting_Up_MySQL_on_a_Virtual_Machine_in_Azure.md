# Instruction

The Nautilus DevOps team needs to deploy a MySQL VM, configure a database, and connect an existing PHP application to it using Azure CLI.

### Requirements

#### MySQL VM

| Setting    | Value                              |
| ---------- | ---------------------------------- |
| VM Name    | `datacenter-mysql-vm`              |
| Region     | `centralus`                        |
| Image      | Percona Server for MySQL (Jetware) |
| Size       | `Standard_B1s`                     |
| Username   | `datacenter_admin`                 |
| Password   | `Namin@123456`                     |
| OS Disk    | Standard HDD                       |
| Open Ports | 22, 3306                           |

#### Database

| Setting  | Value             |
| -------- | ----------------- |
| Database | `datacenter_db`   |
| User     | `datacenter_user` |
| Password | `password123`     |

# Solution

## 1. Get Resource Group

```bash
az group list -o table
```

Example:

```bash
RG_NAME="kml_rg_main-xxxx"
```

<img width="962" height="211" alt="rs grp" src="https://github.com/user-attachments/assets/5ed220a0-401e-40c4-bb70-78f19689130f" />

## 2. Find Percona Image

List Jetware images:

```bash
az vm image list \
  --publisher jetware \
  --all -o table | grep -i percona
```

Typical output:

```text
jetware percona-server-for-mysql ...
```

Get the exact URN:

```bash
az vm image list \
  --publisher jetware \
  --all \
  --query "[?contains(offer,'percona')].[urn]" \
  -o tsv
```

Save the URN returned.

Example:

```bash
IMAGE_URN="jetware:percona-server-for-mysql:8_0:latest"
```

<img width="507" height="251" alt="vm image list" src="https://github.com/user-attachments/assets/14b0647e-5117-4dda-8879-005c289e4fb6" />

## 3. Create MySQL VM

```bash
az vm create \
  --resource-group $RG_NAME \
  --name datacenter-mysql-vm \
  --location centralus \
  --image "$IMAGE_URN" \
  --size Standard_B1s \
  --admin-username datacenter_admin \
  --admin-password 'Namin@123456' \
  --authentication-type password \
  --public-ip-sku Standard \
  --storage-sku Standard_LRS
```

<img width="996" height="542" alt="vm create" src="https://github.com/user-attachments/assets/dc1ba134-05d3-49f4-8fc1-987c8a38b633" />

## 4. Open SSH Port

```bash
az vm open-port \
  --resource-group $RG_NAME \
  --name datacenter-mysql-vm \
  --port 22
```

<img width="620" height="240" alt="vm open port" src="https://github.com/user-attachments/assets/aed248cd-c9af-453d-9a97-2c51c7eb8bb3" />

## 5. Open MySQL Port 3306

Find NSG:

```bash
NIC_NAME=$(az vm show \
  -g $RG_NAME \
  -n datacenter-mysql-vm \
  --query "networkProfile.networkInterfaces[0].id" \
  -o tsv | awk -F/ '{print $NF}')
```

```bash
NSG_NAME=$(az network nic show \
  -g $RG_NAME \
  -n $NIC_NAME \
  --query "networkSecurityGroup.id" \
  -o tsv | awk -F/ '{print $NF}')
```

Create rule:

```bash
az network nsg rule create \
  --resource-group $RG_NAME \
  --nsg-name $NSG_NAME \
  --name Allow-MySQL \
  --priority 200 \
  --direction Inbound \
  --access Allow \
  --protocol Tcp \
  --source-address-prefixes '*' \
  --destination-port-ranges 3306
```

# Configure MySQL

## 6. Get MySQL VM Public IP

```bash
MYSQL_IP=$(az vm show \
  -g $RG_NAME \
  -n datacenter-mysql-vm \
  -d \
  --query publicIps \
  -o tsv)
```

<img width="412" height="136" alt="vm public ip" src="https://github.com/user-attachments/assets/9c6151ec-b28d-4bc0-924c-a883cec7a736" />

## 7. SSH Into MySQL VM

```bash
ssh datacenter_admin@$MYSQL_IP
```

<img width="702" height="297" alt="vm ssh" src="https://github.com/user-attachments/assets/db49f597-00c7-4ab2-92a7-23141003e5ec" />

<img width="751" height="122" alt="install mysql" src="https://github.com/user-attachments/assets/52a93a44-e97b-421b-8bb1-f70d65554b58" />

## 8. Enter MySQL

```bash
sudo /jet/enter mysql
```

## 9. Create Database and User

```sql
CREATE DATABASE datacenter_db;

CREATE USER 'datacenter_user'@'%'
IDENTIFIED BY 'password123';

GRANT ALL PRIVILEGES
ON datacenter_db.*
TO 'datacenter_user'@'%';

FLUSH PRIVILEGES;
```

Verify:

```sql
SHOW DATABASES;
SELECT user,host FROM mysql.user;
```

Exit:

```sql
EXIT;
```

<img width="712" height="517" alt="configure database" src="https://github.com/user-attachments/assets/18b96f06-6687-426c-9bfc-7a7f663aabb8" />

# Configure PHP VM

## 10. Get PHP VM Public IP

```bash
PHP_IP=$(az vm show \
  -g $RG_NAME \
  -n datacenter-php-vm \
  -d \
  --query publicIps \
  -o tsv)
```

## 11. SSH Into PHP VM

```bash
ssh azureuser@$PHP_IP
```

<img width="752" height="281" alt="ssh php" src="https://github.com/user-attachments/assets/859d4377-8818-4af4-b97c-128a5b46ec06" />

## 12. Update db_test.php

```bash
sudo vi /var/www/html/db_test.php
```

Replace connection settings:

```php
$servername = "$MYSQL_IP";
$username = "datacenter_user";
$password = "password123";
$dbname = "datacenter_db";
$port = 3306;

$conn = new mysqli(
    $servername,
    $username,
    $password,
    $dbname,
    $port
);
```

<img width="772" height="395" alt="set php var" src="https://github.com/user-attachments/assets/1b37aac8-51f9-4f2d-bd0b-c6f9968dcc26" />

Save.

## 13. Validate PHP File

```bash
php -l /var/www/html/db_test.php
```

Expected:

```text
No syntax errors detected
```

# Test Connectivity

## 14. Test MySQL Port

From PHP VM:

```bash
nc -zv $MYSQL_IP 3306
```

Expected:

```text
succeeded
```

## 15. Open Browser

Visit:

```text
http://<PHP_VM_PUBLIC_IP>/db_test.php
```

Expected:

```text
Connected successfully
```
<img width="672" height="136" alt="verify connection" src="https://github.com/user-attachments/assets/d4c129e7-a0ef-48fa-8f50-f07b2fdd2938" />

# Quick Validation Commands

### Verify Database

```bash
mysql -u datacenter_user \
-ppassword123 \
-h $MYSQL_IP \
-P 3306
```

Expected:

```text
Welcome to the MySQL monitor
```

### Verify Web Page

```bash
curl http://$PHP_IP/db_test.php
```

Expected:

```text
Connected successfully
```

<img width="672" height="136" alt="verify connection" src="https://github.com/user-attachments/assets/4db0e99d-d101-427a-a9f1-dd438ab1adce" />

## Fun Message

*"Your PHP application and MySQL database are now successfully communicating across Azure VMs 🚀🐘🗄️☁️"*
