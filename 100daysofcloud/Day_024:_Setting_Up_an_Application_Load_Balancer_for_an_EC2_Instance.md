# Instruction

The Nautilus DevOps team is currently working on setting up a simple application on the AWS cloud. They aim to establish an Application Load Balancer (ALB) in front of an EC2 instance where an Nginx server is currently running. While the Nginx server currently serves a sample page, the team plans to deploy the actual application later.

Set up an Application Load Balancer named `nautilus-alb`. 

Create a target group named `nautilus-tg`. 

Create a security group named `nautilus-sg` to open port `80` for the public. 

Attach this security group to the ALB. 

The ALB should route traffic on port `80` to port `80` of the `nautilus-ec2` instance. 

Make appropriate changes in the default security group attached to the EC2 instance if necessary.

# Solution


### Step 1: Create the Security Group (`nautilus-sg`)

1. **Open EC2 Dashboard**: Navigate to **Network & Security** > **Security Groups**.
2. **Create Security Group**: Click **Create security group**.

![1](https://github.com/user-attachments/assets/5be1dd7b-d289-4b5b-9240-b7d85a29f795)

3. **Details**:
* **Name**: `nautilus-sg`
* **Description**: Allow public HTTP traffic.

![2](https://github.com/user-attachments/assets/833b33c3-729c-4b5d-87a0-3cd7523d4fce)

4. **Inbound Rules**: Add a rule for **HTTP** (Port 80) with Source **Anywhere-IPv4 (0.0.0.0/0)**.
5. **Finalize**: Click **Create security group**.
* *Note: Ensure the security group attached to your `nautilus-ec2` also allows inbound traffic on Port 80 from this new `nautilus-sg`.*

![3](https://github.com/user-attachments/assets/794e0231-c927-46bf-8098-dd73c78763c4)


### Step 2: Create the Target Group (`nautilus-tg`)

1. **Navigate to Target Groups**: Under **Load Balancing**, click **Target Groups**.
2. **Create Target Group**: Select **Instances** as the target type.

![4](https://github.com/user-attachments/assets/8b167b2b-a1e5-4d5a-bd65-59bf3a3c266e)

3. **Settings**:
* **Target group name**: `nautilus-tg`
* **Protocol**: HTTP (Port 80).


4. **Register Targets**: Select your `nautilus-ec2` instance. Click **Include as pending below** and click **Create target group**.

![5](https://github.com/user-attachments/assets/f25efef6-acb4-439d-8f30-228bb876078a)

![8](https://github.com/user-attachments/assets/6b54e733-bdc0-40d6-8e9d-5009d1afd4bd)

### Step 3: Create the Application Load Balancer (`nautilus-alb`)

1. **Launch ALB**: Under **Load Balancing**, click **Load Balancers** > **Create load balancer** > **Application Load Balancer**.

![6](https://github.com/user-attachments/assets/d74376b0-87a2-4f7a-b364-9043830ddb1f)

2. **Basic Configuration**:
* **Load balancer name**: `nautilus-alb`
* **Scheme**: Internet-facing.


3. **Network Mapping**:
* Select your VPC.
* **Critical Fix**: Select at least two Availability Zones (AZs). Ensure one of the selected AZs matches the AZ where your `nautilus-ec2` is running to avoid the mismatch issue.


4. **Security Groups**: Select the `nautilus-sg` you created.
5. **Listeners and Routing**:
* Protocol: HTTP (Port 80).
* Default action: Forward to `nautilus-tg`.


6. **Finalize**: Click **Create load balancer**.

![7](https://github.com/user-attachments/assets/3c9d981b-9752-400f-b487-569cea9d981f)

![9](https://github.com/user-attachments/assets/85750e7a-a32d-44c5-8880-0dd39613d941)

![10](https://github.com/user-attachments/assets/807e2b44-6d9f-447b-9642-ebea6d7b2ad0)

---

### Key Technical Considerations

* **Availability Zones**: ALBs require at least two subnets in different AZs to ensure high availability. If your instance is in `us-east-1a`, your ALB must include `us-east-1a` plus at least one other (like `us-east-1b`).
* **Target Health**: After creation, the target status will be **Initial**. It only becomes **Healthy** once the ALB successfully performs a health check on the Nginx service.
* **Security Group Nesting**: For better security, don't open the EC2 to the whole world. Set the EC2’s security group to only accept Port 80 traffic from the ALB’s security group ID.

---

### Fun Fact

> Think of an Application Load Balancer like a receptionist in a busy office building. Visitors (the public) only ever talk to the receptionist at the front desk (the ALB). The receptionist then directs them to the specific office (the EC2 instance) where they need to go, keeping the internal layout of the building private!

[medium](https://medium.com/@hritikraj8804/%EF%B8%8F-day-024-of-100daysofcloud-aws-application-load-balancer-58627662293e)
