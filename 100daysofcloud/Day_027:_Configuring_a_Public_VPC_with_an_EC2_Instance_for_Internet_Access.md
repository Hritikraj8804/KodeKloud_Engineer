# Instruction

The Nautilus DevOps Team has received a request from the Networking Team to set up a new public VPC to support a set of public-facing services. This VPC will host various resources that need to be accessible over the internet. As part of this setup, you need to ensure the VPC has public subnets with automatic IP assignment for resources. Additionally, a new EC2 instance will be launched within this VPC to host public applications that require SSH access. This setup will enable the Networking Team to deploy and manage public-facing applications.

Create a public VPC named `devops-pub-vpc`, and a subnet named `devops-pub-subnet` under the same, make sure public IP is being auto assigned to resources under this subnet. Further, create an EC2 instance named `devops-pub-ec2` under this VPC with instance type `t2.micro`. Make sure SSH port `22` is open for this instance and accessible over the internet.

# Solution


### Step 1: Create the VPC and Subnet

1. **Open VPC Dashboard**: Log in to the AWS Management Console and go to **VPC**.
2. **Create VPC**: Click **Create VPC**.
* **Resources to create**: Select **VPC only**.
* **Name tag**: `devops-pub-vpc`
* **IPv4 CIDR block**: Enter `10.0.0.0/16`.

![1](https://github.com/user-attachments/assets/392628e5-c5f3-480d-91e1-dc56a1f93cd6)

3. **Create Subnet**: In the left menu, click **Subnets** > **Create subnet**.
* **VPC ID**: Select `devops-pub-vpc`.
* **Subnet name**: `devops-pub-subnet`
* **IPv4 CIDR block**: Enter `10.0.1.0/24`.
4. **Enable Auto-Assign IP**: Select your new subnet, click **Actions** > **Edit subnet settings**, and check the box for **Enable auto-assign public IPv4 address**. Save your changes.

![2](https://github.com/user-attachments/assets/89a759dd-094d-49f8-b692-fc165f481b9b)

### Step 2: Establish Internet Connectivity

1. **Create Internet Gateway**: Go to **Internet Gateways** > **Create internet gateway**. Name it `devops-pub-igw`.
2. **Attach to VPC**: Select the gateway, click **Actions** > **Attach to VPC**, and choose `devops-pub-vpc`.

![4](https://github.com/user-attachments/assets/bc64f210-c9cf-40cc-b325-26b26ee433a9)

3. **Configure Route Table**: Go to **Route Tables**, select the table for your VPC, and click **Edit routes**.
* **Add route**: Destination `0.0.0.0/0` and Target **Internet Gateway** (`devops-pub-igw`).
* *This "default route" is what actually makes your subnet "public."*

![3](https://github.com/user-attachments/assets/5bea20e9-364a-45bd-87cb-6492ee1b7544)

### Step 3: Launch the EC2 Instance

1. **Navigate to EC2**: Go to **Instances** > **Launch instances**.
2. **Configuration**:
* **Name**: `devops-pub-ec2`
* **AMI**: Select any **Ubuntu** AMI.
* **Instance type**: `t2.micro`.
3. **Network Settings**: Click **Edit**.
* **VPC**: Select `devops-pub-vpc`.
* **Subnet**: Select `devops-pub-subnet`.
4. **Security Group**: Create a new group.
* **Rule 1 (SSH)**: Set Port **22** and Source to **Anywhere (0.0.0.0/0)** or your specific IP.
5. **Finalize**: Click **Launch instance**.

![5](https://github.com/user-attachments/assets/511f7768-3d63-46ac-97ac-722429c517f0)

![6](https://github.com/user-attachments/assets/2e8d7ab2-2d83-47e2-a275-df5dc4b8ea03)

---

### Key Technical Considerations

* **The IGW Link**: A VPC is just a private bubble until you attach an Internet Gateway and point a route table toward it. Without Step 2, your "public" subnet won't actually have internet access.
* **Security Best Practices**: While opening port 22 to `0.0.0.0/0` makes the instance accessible, it also makes it a target for brute-force attacks. In a production environment, you should limit the source IP to your own network.
* **Public IP Persistence**: Even with auto-assign enabled, the public IP will change if you stop and start the instance. Use an Elastic IP if you need a permanent address.

---

### Fun Fact

> Think of a VPC like a gated community. The **Subnet** is your specific street, and the **Internet Gateway** is the main security gate. If you don't tell the guard (the Route Table) that people are allowed to enter from the outside world (0.0.0.0/0), no one will ever find your house!

👉[Dev.to](https://dev.to/hritikraj8804/aws-127-building-from-scratch-creating-a-custom-public-vpc-19ob)
