# Instruction

The DevOps team is tasked with setting up a highly available web application using AWS. To achieve this, they plan to use an Auto Scaling Group (ASG) to ensure that the required number of EC2 instances are always running, and an Application Load Balancer (ALB) to distribute traffic across these instances. The goal of this task is to set up an ASG that automatically scales EC2 instances based on CPU utilization, and an ALB that directs incoming traffic to the instances. The EC2 instances should have Nginx installed and running to serve web traffic.

Create an EC2 launch template named `devops-launch-template` that specifies the configuration for the EC2 instances, including the `Amazon Linux 2 AMI`, `t2.micro` instance type, and a security group that allows HTTP traffic on port `80`.

Add a User Data script to the launch template to install Nginx on the EC2 instances when they are launched. The script should install Nginx, start the `Nginx` service, and enable it to start on boot.

Create an Auto Scaling Group named `devops-asg` that uses the launch template and ensures a minimum of `1` instance, desired capacity is `1` instance and a maximum of `2` instances are running based on CPU utilization. Set the target CPU utilization to `50%`.

Create a target group named `devops-tg`, an Application Load Balancer named `devops-alb` and configure it to listen on port `80`. Ensure the ALB is associated with the Auto Scaling Group and distributes traffic across the instances.

Configure health checks on the ALB to ensure it routes traffic only to healthy instances.

Verify that the ALB's DNS name is accessible and that it displays the default Nginx page served by the EC2 instances.

# Solution

![6](https://github.com/user-attachments/assets/79ba18cf-1d43-40d3-bbd5-66982a515f1a)

![8](https://github.com/user-attachments/assets/a27aae92-759b-47dd-8fa4-a25d4f8fbe0d)


### **Phase 1: Create the Launch Template (`devops-launch-template`)**

1. Navigate to **EC2 > Launch Templates** and click **Create launch template**.
2. **Name:** `devops-launch-template`.
3. **Application and OS Images:** Choose **Amazon Linux 2 AMI**.
4. **Instance type:** Select **t2.micro**.
5. **Network settings:** Select a security group that allows **Inbound HTTP (Port 80)** from `0.0.0.0/0`.
6. **Advanced details:** Scroll to the bottom to **User data** and paste:
```bash
#!/bin/bash
sudo yum update -y
sudo amazon-linux-extras install nginx1 -y
sudo systemctl start nginx
sudo systemctl enable nginx
```
7. Click **Create launch template**.

![1](https://github.com/user-attachments/assets/2fcc6a13-33c0-41c3-b6bb-94a922bc6a5c)

---

### **Phase 2: Create the Target Group and ALB**

Before creating the ASG, we need the "destination" for the traffic.

1. **Target Group:** Go to **EC2 > Target Groups > Create target group**.
* **Type:** Instances.
* **Name:** `devops-tg`.
* **Protocol/Port:** HTTP / 80.
* **VPC:** Select your default VPC.
* Click **Next** and then **Create target group** (don't register instances yet).

![2](https://github.com/user-attachments/assets/9f4fa1ae-b5c2-409e-986f-7327761ce0a9)

2. **Load Balancer:** Go to **EC2 > Load Balancers > Create load balancer**.
* Choose **Application Load Balancer**.
* **Name:** `devops-alb`.
* **Network mapping:** Select at least two Availability Zones.
* **Listeners and routing:** Protocol HTTP, Port 80, Forward to **`devops-tg`**.
* Click **Create load balancer**.

![3](https://github.com/user-attachments/assets/cffdfb52-eed4-41c3-8091-440236c8cc7e)

---

### **Phase 3: Create the Auto Scaling Group (`devops-asg`)**

1. Go to **EC2 > Auto Scaling Groups > Create Auto Scaling group**.
2. **Name:** `devops-asg`. Select your `devops-launch-template`. Click **Next**.
3. **Network:** Select your VPC and the same subnets used for the ALB. Click **Next**.
4. **Load balancing:** Select **Attach to an existing load balancer**. Choose your **`devops-tg`**.
5. **Group size:** * **Desired:** 1 | **Minimum:** 1 | **Maximum:** 2.
6. **Scaling policies:** Select **Target tracking scaling policy**.
* **Metric type:** Average CPU utilization.
* **Target value:** 50.
7. Complete the wizard and click **Create Auto Scaling group**.

![4](https://github.com/user-attachments/assets/39c9f3a8-710a-4287-a25c-c1d5aedd3524)

![5](https://github.com/user-attachments/assets/1c7aa2d3-a067-43b5-a81f-826540e05dbd)

---

### **Phase 4: Verification**

1. **Wait:** Give the ASG 2–3 minutes to launch the first instance.
2. **Check Health:** Go to **Target Groups > devops-tg > Targets**. The status should move from `initial` to `healthy`.
3. **Test DNS:** Go to **Load Balancers**, copy the **DNS name** of `devops-alb`, and paste it into your browser. You should see the "Welcome to nginx!" page.

![7 1](https://github.com/user-attachments/assets/e4708f76-32c6-445d-bd79-292ab79b5d67)

---

👉[Dev.to](https://dev.to/hritikraj8804/aws-144-scaling-for-success-high-availability-with-asg-and-alb-2l9p)
