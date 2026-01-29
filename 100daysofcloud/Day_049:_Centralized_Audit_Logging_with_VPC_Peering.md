# Instruction

use console

The Nautilus DevOps team needs to build a secure and scalable log aggregation setup within their AWS environment. The goal is to gather log files from an internal EC2 instance running in a private VPC, transfer them securely to another EC2 instance in a public VPC, and then push those logs to a secure S3 bucket.

1) A VPC named `devops-priv-vpc` already exists with a private subnet named `devops-priv-subnet`, a route table named `devops-priv-rt`, and an EC2 instance named `devops-priv-ec2` (using ubuntu image). This instance uses the SSH key pair `devops-key.pem` already available on the AWS client host at `/root/.ssh/`.

2) Your task is to:

Create a new VPC named `devops-pub-vpc`.

Create a subnet named `devops-pub-subnet` and a route table named `devops-pub-rt` under this public VPC.

Attach an internet gateway to `devops-pub-vpc` and configure the public route table to enable internet access.

Launch an EC2 instance named `devops-pub-ec2` into the public subnet using the same key pair as the private instance.

Create an IAM role named `devops-s3-role` with PutObject permission to an S3 bucket and attach it to the public EC2 instance.

Create a new private S3 bucket named `devops-s3-logs-28057`.

Configure a VPC Peering named `devops-vpc-peering` between the private and public VPCs.

Modify both `devops-priv-rt` and `devops-pub-rt` to route each other's CIDR blocks through the peering connection.

On the private instance, configure a cron job to push the `/var/log/boots.log` file to the public instance (using scp or rsync).

On the public instance, configure a cron job to push that same file to the created S3 bucket.

The uploaded file must be stored in the S3 bucket under the path `devops-priv-vpc/boot/boots.log`.


# Solution
---
### **Step 1: Public Networking (VPC, Subnet, IGW)**

1. **VPC:** Create VPC named `xfusion-pub-vpc`. Note its CIDR (e.g., `10.1.0.0/16`).
2. **Subnet:** Create `xfusion-pub-subnet` in the new VPC (e.g., `10.1.1.0/24`).
3. **IGW:** Create an Internet Gateway, attach it to `xfusion-pub-vpc`.
4. **Route Table:** * Create `xfusion-pub-rt`.
* **Edit Routes:** Add `0.0.0.0/0` with Target = Internet Gateway.
* **Subnet Association:** Associate `xfusion-pub-rt` with `xfusion-pub-subnet`.



### **Step 2: Security Groups & Public Instance**

1. **Security Group (Public):** Create one that allows:
* **SSH (22)** from your IP.
* **SSH (22)** from the private VPC CIDR (`xfusion-priv-vpc`).


2. **EC2:** Launch `xfusion-pub-ec2` (Ubuntu) in `xfusion-pub-subnet`.
* Use the existing key pair `xfusion-key`.
* Ensure **Auto-assign Public IP** is enabled.



### **Step 3: S3 & IAM Role**

1. **S3:** Create bucket `xfusion-s3-logs-5462`. Keep it private.
2. **IAM Role:**
* Create role `xfusion-s3-role` for **EC2**.
* Attach a policy with `s3:PutObject` permission for that specific bucket.
* **Attach Role:** Go to EC2 console > `xfusion-pub-ec2` > Actions > Security > **Modify IAM Role** and select `xfusion-s3-role`.



### **Step 4: VPC Peering (The "Easy to Miss" Step)**

1. **Create Peering:** `xfusion-vpc-peering` from `xfusion-priv-vpc` to `xfusion-pub-vpc`.
2. **Acceptance:** You **must** go to the Peering Connections console and click **Actions > Accept Request**.
3. **Update Route Tables:**
* **In `xfusion-pub-rt`:** Add a route to the **Private VPC CIDR** via the Peering Connection.
* **In `xfusion-priv-rt`:** Add a route to the **Public VPC CIDR** via the Peering Connection.


![13](https://github.com/user-attachments/assets/fb4b1eb4-59c2-49cb-ae8a-871cb1d1fc1c)

---

### **Step 5: File Transfer Logic (Private to Public)**

Login to the **Private Instance** (use the client host to jump or use the `.pem` file).

1. **SSH Prep:** The private instance needs the `xfusion-key.pem` to talk to the public one. Copy it there or use SSH Agent forwarding.
2. **Test SCP:** Manually run this to ensure connectivity:
`scp -i ~/.ssh/xfusion-key.pem /var/log/boot.log ubuntu@<PUBLIC_INSTANCE_PRIVATE_IP>:/tmp/boots.log`
3. **Cron Job:**
`crontab -e`
Add: `* * * * * scp -o StrictHostKeyChecking=no -i /root/.ssh/xfusion-key.pem /var/log/boot.log ubuntu@<PUBLIC_INSTANCE_PRIVATE_IP>:/home/ubuntu/boots.log`

### **Step 6: S3 Upload Logic (Public to S3)**

Login to the **Public Instance**.

1. **AWS CLI:** Ensure AWS CLI is installed.
2. **Test S3 Upload:**
`aws s3 cp /home/ubuntu/boots.log s3://xfusion-s3-logs-5462/xfusion-priv-vpc/boot/boots.log`
3. **Cron Job:**
`crontab -e`
Add: `* * * * * aws s3 cp /home/ubuntu/boots.log s3://xfusion-s3-logs-5462/xfusion-priv-vpc/boot/boots.log`

![14](https://github.com/user-attachments/assets/e00a0dda-ebf6-494f-9816-5526376e3b21)

---

### **Critical "Don't Miss" Checklist:**

* **Permissions:** Did you apply `chmod 400` to the `.pem` file on both instances?
* **S3 Path:** Is it exactly `xfusion-priv-vpc/boot/boots.log`? (Case sensitive!)
* **VPC Peering:** Did you accept the request AND update **both** route tables?
* **Security Groups:** Does the public instance allow SSH from the private instance's internal IP?


👉[Dev.to](https://dev.to/hritikraj8804/aws-149-multi-vpc-log-aggregation-peering-iam-roles-and-s3-integration-666)
