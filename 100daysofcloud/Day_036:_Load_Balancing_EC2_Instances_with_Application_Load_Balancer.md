# Instruction

The Nautilus Development Team needs to set up a new EC2 instance and configure it to run a web server. This EC2 instance should be part of an Application Load Balancer (ALB) setup to ensure high availability and better traffic management. The task involves creating an EC2 instance, setting up an ALB, configuring a target group, and ensuring the web server is accessible via the ALB DNS.

Create a security group: Create a security group named `xfusion-sg` to open port `80` for the default security group (which will be attached to the ALB). Attach `xfusion-sg` security group to the EC2 instance.

Create an EC2 instance: Create an EC2 instance named `xfusion-ec2`. Use any available `Ubuntu` AMI to create this instance. Configure the instance to run a user data script during its launch.

This script should:

Install the `Nginx` package.

Start the Nginx service.

Set up an Application Load Balancer: Set up an Application Load Balancer named `xfusion-alb`. Attach default security group to the same.

Create a target group: Create a target group named `xfusion-tg`.

Route traffic: The ALB should route traffic on port `80` to port `80` of the `xfusion-ec2` instance.

Security group adjustments: Make appropriate changes in the default security group attached to the ALB if necessary. Eventually, the Nginx server running under `xfusion-ec2` instance must be accessible using the ALB DNS.

# Solution

#### Step 1: Create the Instance Security Group (`xfusion-sg`)

1. Navigate to **EC2 > Security Groups > Create security group**.
2. **Name:** `xfusion-sg`.
3. **VPC:** Ensure the **Default VPC** is selected.
4. **Inbound Rules:**
* **Type:** HTTP.
* **Port Range:** 80.
* **Source:** Select **Custom** and choose the **default** security group ID.
5. Click **Create security group**.

![1](https://github.com/user-attachments/assets/50c54144-4328-497e-a2b8-0281b074daef)

#### Step 2: Launch the EC2 Instance (`xfusion-ec2`)

1. Go to **EC2 > Instances > Launch instances**.
2. **Name:** `xfusion-ec2`.
3. **AMI:** Select **Ubuntu** (e.g., Ubuntu Server 24.04 LTS).
4. **Instance type:** `t2.micro` or `t3.micro`.
5. **Network settings:** * Select the **xfusion-sg** security group.
6. **Advanced details > User data:** Paste the following script:
```bash
#!/bin/bash
apt-get update -y
apt-get install nginx -y
systemctl start nginx
systemctl enable nginx

```
7. Click **Launch instance**.

![2](https://github.com/user-attachments/assets/7155af7d-a158-44c0-894f-a00b1db2ba15)
#### Step 3: Create the Target Group (`xfusion-tg`)

1. Navigate to **EC2 > Target Groups > Create target group**.
2. **Target type:** Instances.
3. **Target group name:** `xfusion-tg`.
4. **Protocol/Port:** HTTP / 80.
5. Click **Next**.
6. **Register targets:** Select `xfusion-ec2`, click **Include as pending below**, then click **Create target group**.

![3](https://github.com/user-attachments/assets/7c3049d0-a163-4aad-b1d9-0feefec5637f)

#### Step 4: Create the Application Load Balancer (`xfusion-alb`)

1. Navigate to **EC2 > Load Balancers > Create load balancer**.
2. Select **Application Load Balancer**.
3. **Name:** `xfusion-alb`.
4. **Network mapping:** Select the default VPC and at least two Availability Zones.
5. **Security groups:** Select the **default** security group.
6. **Listeners and routing:** * Protocol: HTTP, Port: 80.
* Default action: Forward to **xfusion-tg**.
7. Click **Create load balancer**.

![4](https://github.com/user-attachments/assets/459d720e-c659-4bb7-a8c9-d5a17f5f6a22)

![5](https://github.com/user-attachments/assets/23225267-65b8-4592-9d9c-a563d6718a18)

#### Step 5: Adjust Default Security Group (ALB)

1. Go to **Security Groups** and find the **default** security group.
2. **Edit inbound rules:** * Add **HTTP (80)** from source `0.0.0.0/0`. This allows the public to reach your ALB.
3. Click **Save rules**.

#### Step 6: Verification

1. Go to **EC2 > Load Balancers** and select `xfusion-alb`.
2. Copy the **DNS name** (e.g., `xfusion-alb-123456789.us-east-1.elb.amazonaws.com`).
3. Paste the DNS name into your browser. You should see the "Welcome to nginx!" default page.

![7](https://github.com/user-attachments/assets/fa6a8831-b583-4ac8-ac03-8e7f2df0a6ad)

---

### Challenges

* **Health Check Failures:** If the ALB shows the target as `unhealthy`, ensure the Nginx service has finished installing. It can take 1–2 minutes after launch for the User Data script to complete.
![6](https://github.com/user-attachments/assets/d3796852-7b0f-4bfe-af04-35237c6b07c9)

* **AZ Mapping:** Ensure your ALB is mapped to subnets in at least two different Availability Zones. If you only select one, the ALB creation will fail.
* **Implicit Inbound Rules:** The `xfusion-sg` must allow port 80 specifically from the security group of the ALB. If you use a different SG for the ALB later, you must update the source in `xfusion-sg`.

---

### Funny Message

> Your traffic is now being expertly balanced! It’s like a digital traffic cop directing data packets to their destination without a single traffic jam. Now Nginx can serve up web pages while the ALB handles the heavy lifting teamwork makes the dream work!

👉[Dev.to](https://dev.to/hritikraj8804/aws-136-high-availability-architecture-load-balancing-nginx-with-alb-39c5)
