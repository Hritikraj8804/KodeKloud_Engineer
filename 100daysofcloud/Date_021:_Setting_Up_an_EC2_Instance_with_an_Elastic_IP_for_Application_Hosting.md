# Instruction

The Nautilus DevOps Team has received a new request from the Development Team to set up a new EC2 instance. This instance will be used to host a new application that requires a stable IP address. To ensure that the instance has a consistent public IP, an Elastic IP address needs to be associated with it. The instance will be named `devops-ec2`, and the Elastic IP will be named `devops-eip`. This setup will help the Development Team to have a reliable and consistent access point for their application.
Create an EC2 instance named `devops-ec2` using any linux AMI like ubuntu, the Instance type must be `t2.micro` and associate an Elastic IP address with this instance, name it as `devops-eip`.

# Solution

Building a stable access point is a vital part of application deployment. By using an Elastic IP (EIP), you ensure that the application's public entry point remains constant even if the underlying virtual machine is stopped or restarted.

Below are the steps to set up the `devops-ec2` instance and link it with the `devops-eip`.

---

### Step 1: Launch the EC2 Instance

1. **Open EC2 Dashboard**: Log in to the AWS Management Console and navigate to **EC2**.
2. **Launch Instance**: Click **Launch instance**.

![4](https://github.com/user-attachments/assets/af91c8e9-e1cb-41e4-b829-f2b4287e5957)

3. **Name and Tags**:
* **Name**: `devops-ec2`
4. **Application and OS Image (AMI)**:
* Select **Ubuntu** (or any Linux distribution of your choice).
* Ensure a Free Tier eligible version is selected (e.g., Ubuntu Server 24.04 LTS).
5. **Instance Type**:
* Select **t2.micro**.
6. **Key Pair**: Select an existing key pair or create a new one to ensure you can SSH into the instance later.
7. **Network Settings**: Keep the default VPC and Subnet. Ensure **Auto-assign public IP** is enabled (though the EIP will eventually replace this).
8. **Launch**: Click **Launch instance**.

![5](https://github.com/user-attachments/assets/2f83e149-e465-4d8a-8235-cc0706d0c211)

---

### Step 2: Allocate the Elastic IP

1. **Navigate to Elastic IPs**: In the left-hand menu of the EC2 Dashboard, under **Network & Security**, click **Elastic IPs**.

![6](https://github.com/user-attachments/assets/05495339-bcfb-469d-a389-bf79b040370f)

2. **Allocate IP**: Click the **Allocate Elastic IP address** button.

![7](https://github.com/user-attachments/assets/73a0d1f1-17c2-4fa3-9c2f-943509e2d0f4)

3. **Settings**: Leave the default settings (**Amazon's pool of IPv4 addresses**).
4. **Add Tag**:
* Click **Add new tag**.
* **Key**: `Name`
* **Value**: `devops-eip`

![8](https://github.com/user-attachments/assets/08d663c6-8e74-48f3-8442-184e299f723b)

5. **Finalize**: Click **Allocate**.

---

### Step 3: Associate the Elastic IP with the Instance

1. **Select IP**: Select the checkbox next to the newly created `devops-eip`.
2. **Associate**: Click the **Actions** button and select **Associate Elastic IP address**.

![9](https://github.com/user-attachments/assets/ba1e598d-4498-4338-83c6-09194d074a97)

3. **Resource Type**: Choose **Instance**.
4. **Instance**: Search for and select the **devops-ec2** instance you just created.
5. **Private IP**: Choose the private IP address of the instance that appears in the dropdown.
6. **Finalize**: Click **Associate**.

![10](https://github.com/user-attachments/assets/a79016fc-f74c-42cc-b741-edbcd2773278)

![11](https://github.com/user-attachments/assets/887a5f85-e3da-476e-aea7-61d561ec9422)

---

### Key Technical Considerations

* **Cost Management**: AWS provides one Elastic IP for free as long as it is **associated** with a running instance. If the instance is terminated or stopped, you may incur a small hourly charge for the unattached IP.
* **Security Groups**: Even with a static IP, you must ensure your Instance's Security Group allows traffic on the specific ports your application uses (e.g., Port 80 for HTTP or Port 22 for SSH).
* **DNS Updates**: If the Development Team uses a domain name (like `app.nautilus.com`), they should point their DNS "A Record" to this new `devops-eip`.

---

### Fun Fact

> Think of an Elastic IP like a "permanent phone number." Even if you upgrade your phone (the EC2 instance), your friends (the developers) can still reach you using the same number without having to update their contact lists!

