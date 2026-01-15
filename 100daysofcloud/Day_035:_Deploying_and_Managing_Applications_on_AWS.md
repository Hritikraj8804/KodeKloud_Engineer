# Instruction

The Nautilus DevOps team needs a new private RDS instance for their application. They need to set up a MySQL database and ensure that their existing EC2 instance can connect to it. This will help in managing their database needs efficiently and securely.

1) Task Details:

  Create a private RDS instance named `devops-rds` using a sandbox template.
  
  The engine type must be MySQL v8.4.5, and it must be a `db.t3.micro` type instance.
  
  The master username must be `devops_admin` with an appropriate password.
  
  The RDS storage type must be `gp2`, and the storage size must be `5GiB`.
  
  Create a database named `devops_db`.
  
  Keep the rest of the configurations as default. Ensure the instance is in available state.

Adjust the security groups so that the `devops-ec2` instance can connect to the RDS on port `3306` and also open port `80` for the instance.

2) An EC2 instance named `devops-ec2` exists. Connect to this instance from the AWS console. Create an SSH key (`/root/.ssh/id_rsa`) on the `aws-client` host if it doesn't already exist. Add the public key to the authorized keys of the root user on the EC2 instance for password-less SSH access.

3) There is a file named `index.php` under the `/root` directory on the `aws-client host`. Copy this file to the `devops-ec2` instance under the `/var/www/html/` directory. Make the appropriate changes in the file to connect to the RDS.

4) You should see a Connected successfully message in the browser once you access the instance using the public IP.

# Solution

#### Step 1: Create the RDS Instance

1. Navigate to **RDS > Databases > Create database**.
2. Choose **Standard create** and **MySQL**. Select version **8.4.5**.
3. In **Templates**, select **Free tier** (which uses the Sandbox configuration).
4. Set **DB instance identifier** to `devops-rds`.
5. Set **Master username** to `devops_admin` and set your password.
6. For **Instance configuration**, select **`db.t3.micro`**.
7. For **Storage**, choose **General Purpose SSD (gp2)** and set **Allocated storage** to **5 GiB**.
8. In **Connectivity**, set **Public access** to **No**.
9. In **Additional configuration**, enter `devops_db` for **Initial database name**.
10. Click **Create database**. Wait for status **Available**.

![1](https://github.com/user-attachments/assets/662bc78e-0a1b-4ce9-bf9a-053fb3da0772)

![2](https://github.com/user-attachments/assets/7faea35b-dc25-4bd0-98e0-db6258055081)

#### Step 2: Configure Security Groups

1. Go to **EC2 > Security Groups**.
2. **EC2 SG**: Edit inbound rules for the `devops-ec2` security group. Add **HTTP (80)** from source `0.0.0.0/0`.
3. **RDS SG**: Edit inbound rules for the RDS security group. Add **MySQL/Aurora (3306)** and select the **security group ID** of `devops-ec2` as the source.

![3](https://github.com/user-attachments/assets/5ff449b2-6b1b-4a66-81ca-57569559aa17)

#### Step 3: Setup Password-less SSH

On the `aws-client` host:

1. Check for key: `ls /root/.ssh/id_rsa`.
2. If missing, run: `ssh-keygen -t rsa -N "" -f /root/.ssh/id_rsa`.
3. Copy public key to EC2: `ssh-copy-id -i /root/.ssh/id_rsa.pub root@<EC2_Public_IP>`.
*(Note: Ensure root SSH is enabled on the instance).*

#### Step 4: Deploy PHP Application

1. Copy the file: `scp /root/index.php root@<EC2_Public_IP>:/var/www/html/`.

  ![5](https://github.com/user-attachments/assets/aeb1f21d-1a30-49ce-96c8-a79e36978470)

3. SSH into the EC2 and edit `/var/www/html/index.php`:
* Update `$servername` with the **RDS Endpoint** (found in RDS Console).
* Update `$username` to `devops_admin`.
* Update `$password` and `$dbname` (`devops_db`).

  ![4](https://github.com/user-attachments/assets/f4854d6e-938c-41c1-813c-65933d77e12d)

3. Restart your web server (e.g., `sudo systemctl restart httpd` or `apache2`).

#### Step 5: Verification

Access the public IP of `devops-ec2` in your browser. You should see: **"Connected successfully"**.

  ![res](https://github.com/user-attachments/assets/0a038afb-6e7a-4e3c-864b-0734ee9ac8d7)

---

### Challenges

* **Private Subnet Accessibility:** If your EC2 and RDS are in different VPCs or subnets without a route, they won't talk.
* **Security Group Source:** Using an IP instead of a Security Group ID for port 3306 is a common mistake that breaks security best practices.
* **Root SSH Permissions:** Many default AMIs disable root SSH by default (`PermitRootLogin`). You may need to edit `/etc/ssh/sshd_config`.

---

### Funny Message

> Your database is now officially "private," which in IT terms means it's an introvert that only likes talking to your application server. If you get a "Connected successfully" message, you've officially successfully played matchmaker!

👉[Dev.to](https://dev.to/hritikraj8804/aws-135-the-full-stack-connecting-ec2-to-a-private-rds-mysql-database-ok0)
