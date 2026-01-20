# Instruction

The Nautilus Development Team recently deployed a new web application hosted on an EC2 instance within a public VPC named datacenter-vpc. The application, running on an Nginx server, should be accessible from the internet on port 80. Despite configuring the security group datacenter-sg to allow traffic on port 80 and verifying the EC2 instance settings, the application remains inaccessible from the internet. The team suspects that the issue might be related to the VPC configuration, as all other components appear to be set up correctly. The DevOps team has been asked to troubleshoot and resolve the issue to ensure the application is accessible to external users.
As a member of the Nautilus DevOps Team, your task is to perform the following:

Verify VPC Configuration: Ensure that the VPC `datacenter-vpc `is properly configured to allow internet access.

Ensure Accessibility: Make sure the EC2 instance `datacenter-ec2` running the Nginx server is accessible from the internet on port `80`.

# Solution


---

### **The Connectivity Path Audit**

To ensure **datacenter-ec2** is reachable, you must verify the core components of a "Public Subnet" setup:

| Component | Status Check | Action Required |
| --- | --- | --- |
| **Internet Gateway (IGW)** | Attached to `datacenter-vpc`? | If missing, create and attach one. |
| **Route Table** | Entry for `0.0.0.0/0` -> `igw-xxxx`? | Add a default route pointing to your IGW. |
| **Public IP Address** | Does the instance have one? | Ensure a Public IPv4 or Elastic IP is assigned. |
| **Network ACL** | Inbound/Outbound Port 80 allowed? | Verify stateless rules permit bidirectional traffic. |

---

### **Step 1: Verify VPC Internet Gateway**

A VPC cannot communicate with the internet without an Internet Gateway (IGW).

1. Navigate to the **VPC Console > Internet Gateways**.
2. Check if an IGW is associated with `datacenter-vpc`.
3. **If not found**:
* Click **Create internet gateway**, name it `datacenter-igw`.
* Select it, click **Actions > Attach to VPC**, and choose `datacenter-vpc`.

![1](https://github.com/user-attachments/assets/4ea1201b-d4e4-403f-9917-211f953eea67)


---

### **Step 2: Configure the Public Route Table**

For a subnet to be "public," its route table must explicitly direct internet-bound traffic to the IGW.

1. Go to **VPC Console > Route Tables**.
2. Find the route table associated with the subnet for `datacenter-ec2`.
3. Select the **Routes** tab and click **Edit routes**.
4. **Add route**:
* **Destination**: `0.0.0.0/0`
* **Target**: **Internet Gateway** (select your `datacenter-igw`).
5. Click **Save changes**.

![2 1](https://github.com/user-attachments/assets/d0e2b6c3-4836-40cb-9062-5b9d1fd8321c)

---

### **Step 3: Ensure Public IPv4 Assignment**

Even with correct routing, the internet cannot "find" your instance without a public address.

1. Navigate to the **EC2 Console > Instances** and select `datacenter-ec2`.
2. Check the **Public IPv4 address** field.
3. **If empty**:
* Go to **Network & Security > Elastic IPs**.
* Click **Allocate Elastic IP address**, then **Allocate**.
* Select the new IP, click **Actions > Associate Elastic IP address**.
* Choose `datacenter-ec2` and click **Associate**.

---

### **Step 4: Final Accessibility Test**

1. Confirm Nginx is running: SSH into the instance and run `sudo systemctl status nginx`.
2. Open your browser and enter the **Public IPv4 address**: `http://<Your-Public-IP>`.
3. You should now see the "Welcome to nginx!" default page.

![3](https://github.com/user-attachments/assets/435b36ce-7bf5-4f15-891d-9595f085bab8)

---

### **Verify**

![4](https://github.com/user-attachments/assets/2798a039-b64b-464d-84bd-724349c54382)

![2 5](https://github.com/user-attachments/assets/e2d67eef-bf62-45cb-a03b-e06627ed635f)

![2 6](https://github.com/user-attachments/assets/6d6cfcfe-22ae-4749-b214-1075dc2a49dd)

![2 7](https://github.com/user-attachments/assets/1954c570-8a84-4c7e-9089-417717d2e717)

![2 8](https://github.com/user-attachments/assets/404287b7-a993-4022-8fe2-2ddc07de6ce6)

### **Why this Troubleshooting works**

* **Layered Security**: Security Groups act as a firewall at the instance level, but Route Tables and IGWs handle the "highway" that gets traffic to the gate in the first place.
* **Stateless vs. Stateful**: While Security Groups remember allowed connections, Network ACLs are stateless—ensure your NACL also allows the return traffic on ephemeral ports ().


👉[Dev.to](https://dev.to/hritikraj8804/aws-140-vpc-troubleshooting-restoring-internet-connectivity-n1a)
