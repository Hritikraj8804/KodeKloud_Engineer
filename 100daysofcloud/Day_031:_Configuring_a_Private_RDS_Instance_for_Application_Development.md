# Instruction

The Nautilus Development Team is working on a new application feature that requires a reliable and scalable database solution. To facilitate development and testing, they need a new private RDS instance. This instance will be used to store critical application data and must be provisioned using the AWS free tier to minimize costs during the initial development phase. The team has chosen MySQL as the database engine due to its compatibility with their existing systems. The DevOps team has been tasked with setting up this RDS instance, ensuring that it is correctly configured and available for use by the development team.

As a member of the Nautilus DevOps Team, your task is to perform the following:

Provision a Private RDS Instance: Create a new private RDS instance named `devops-rds` using a sandbox template, further it must be a `db.t3.micro` type instance.

Engine Configuration: Use the MySQL engine with version `8.4.x.`

Enable Storage Autoscaling: Enable storage autoscaling and set the threshold value to 50GB. Keep the rest of the configurations as default.

Instance Availability: Ensure the instance is in the available state before submitting this task.


# Solution

### **Database Architecture Overview**

In a private RDS setup, your database resides in a private subnet. Application servers (like EC2 instances in a public subnet) communicate with it over the internal AWS network.

---

### **Configuration Summary for devops-rds**

| Setting | Configuration Value |
| --- | --- |
| **DB Instance Identifier** | `devops-rds` |
| **Engine** | MySQL Community |
| **Version** | `8.4.x` (LTS version) |
| **Template** | **Sandbox** (This allows for Free Tier configurations) |
| **Instance Class** | `db.t3.micro` |
| **Public Access** | **No** (Ensures instance is private) |
| **Storage Autoscaling** | **Enabled** with a **50GB** threshold |

---

### **Key Technical Deep-Dive**

#### **1. Why MySQL 8.4?**

MySQL 8.4 is the latest **Long-Term Support (LTS)** release. It focuses on stability and security, making it ideal for the "critical application data" mentioned by your development team. It includes improved SHA-256 password hashing by default.

#### **2. Storage Autoscaling Mechanics**

By setting the threshold to **50GB**, you are telling AWS: *"If my database grows and has less than 10% of its allocated storage remaining, automatically increase the size."* * **Cost benefit**: You only pay for the extra storage when it’s actually provisioned.

* **Safety net**: It prevents "Storage Full" errors that could crash your application.

#### **3. Ensuring Private Connectivity**

When you select **Public access: No**, RDS does not assign a public IP address to the instance. To connect to it, the development team will need to:

* Use a **Bastion Host** (Jump box) in a public subnet.
* Or use **AWS Client VPN** to bridge into the VPC.

---

### **Verification Checklist**

Before submitting the task, check the **Databases** list in the RDS Console. The instance must show **Available** in green. If it says *Creating*, *Backing-up*, or *Modifying*, the task is not yet complete.

![2](https://github.com/user-attachments/assets/02a31eed-b57d-4e17-b453-916fe6a056be)

👉 [Dev.to](https://dev.to/hritikraj8804/aws-131-scalable-data-provisioning-private-rds-with-storage-autoscaling-1che)
