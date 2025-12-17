# Instruction

The devops DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.
For this task, create an EC2 instance with following requirements:
1) The name of the instance must be devops-ec2.
2) You can use the Amazon Linux AMI to launch this instance.
3) The Instance type must be t2.micro.
4) Create a new RSA key pair named devops-kp.
5) Attach the default (available by default) security group.

# Solution

Follow these steps using the AWS Management Console to launch the required EC2 instance:

#### Step 1: Navigate to the EC2 Dashboard

1. Log into the **AWS Management Console**.
![0](https://github.com/user-attachments/assets/805dbed6-bef7-4e84-ab9a-770867f8af8c)

2. Ensure your region is set to **us-east-1 (N. Virginia)**.
3. Search for and select **EC2** from the Services list.

![1](https://github.com/user-attachments/assets/c319dcf9-15fe-471a-bd02-0fad0bd83656)


#### Step 2: Launch Instance Wizard

![2](https://github.com/user-attachments/assets/02181d9a-a53f-401e-b994-7d54db578c0e)

1. Click the **Launch instances** button.

![3](https://github.com/user-attachments/assets/c1692808-dd61-4ecf-a116-000f4ea286cb)

#### Step 3: Name and Application/OS Image (AMI)

1. Under **Name and tags**, enter the required name: `devops-ec2`.
2. Under **Application and OS Images (Amazon Machine Image)**, select an **Amazon Linux AMI** (e.g., select **Amazon Linux 2 AMI (HVM)** or similar from the Quick Start list).

![4](https://github.com/user-attachments/assets/56c2f60d-bf15-457b-9fdf-5776f88392ae)

#### Step 4: Instance Type

1. Under **Instance type**, select **t2.micro** from the dropdown menu.

![5](https://github.com/user-attachments/assets/bad497b8-8a83-49b5-956b-014ca2268cd2)

#### Step 5: Key Pair Creation

1. Under **Key pair (login)**, select **Create a new key pair**.

![6](https://github.com/user-attachments/assets/dc7404ab-a06c-4ed3-aa28-6100a05f4125)

2. In the pop-up window:
* **Key pair name:** Enter `devops-kp`.
* **Key pair type:** Select **RSA**.
* **Private key file format:** Select **.pem**.

![7](https://github.com/user-attachments/assets/625a2387-05be-4be2-b1cd-b9958095de38)

3. Click **Create key pair**. The private key file (`devops-kp.pem`) will automatically download. **Store it securely.**

#### Step 6: Network Settings

1. Under **Network settings**, click **Edit**.
* **VPC:** Ensure the **Default VPC** is selected.
* **Security group:** Select the **Select existing security group** option.
* In the list of security groups, select the security group named **default** (it will typically have a description like "default VPC security group").

![8](https://github.com/user-attachments/assets/5b491898-18d2-4022-8262-6d9514d7b8b3)


#### Step 7: Storage and Launch

1. Under **Configure storage**, keep the default settings (usually 8 GiB gp2 or gp3 volume) unless otherwise specified.
2. Review all settings to ensure they match the requirements (Name: `devops-ec2`, AMI: Amazon Linux, Type: `t2.micro`, Key Pair: `devops-kp`, Security Group: `default`).

![9](https://github.com/user-attachments/assets/13a96d92-265b-4b86-8abf-0247d95c2155)

3. Click the **Launch instance** button.

#### Step 8: Verification

1. Click **View all instances**.
2. Confirm that an instance named `devops-ec2` is now listed and its **Instance state** is `running` (or pending).

![10](https://github.com/user-attachments/assets/226a1aa7-5f13-4e6d-872d-3637f7513aaf)

---

### Challenges

Here are some potential issues or common mistakes to watch out for:

* **Instance State:** The instance may take a minute or two to transition from `pending` to `running`.
* **Key Pair Loss:** If you lose the `devops-kp.pem` file, you cannot SSH into the instance.
* **Security Group:** Although the task requires the `default` SG, remember that the default SG typically allows all outbound traffic but only limited (or no) inbound traffic. You might need to adjust the rules later if you need to access it via HTTP/SSH from specific IPs.
* **Billing:** The `t2.micro` is usually within the free tier, but running multiple instances for extended periods may incur minor charges if the free tier limit is exceeded.

---

### Fun Message

> Behold, your very own cloud server! It's fast, it's micro, and it's probably running better than the coffee machine in the office. Just don't ask it to solve world peace—it's only a t2.micro, after all!

👉[Dev.to](https://dev.to/hritikraj8804/aws-106-the-moment-of-truth-launching-your-first-ec2-instance-2m3p)
