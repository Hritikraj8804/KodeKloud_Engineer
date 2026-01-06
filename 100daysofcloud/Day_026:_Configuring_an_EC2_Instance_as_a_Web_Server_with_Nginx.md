# Instruction

The Nautilus DevOps Team is working on setting up a new web server for a critical application. The team lead has requested you to create an EC2 instance that will serve as a web server using Nginx. This instance will be part of the initial infrastructure setup for the Nautilus project. Ensuring that the server is correctly configured and accessible from the internet is crucial for the upcoming deployment phase.
As a member of the Nautilus DevOps Team, your task is to create an EC2 instance with the following specifications:

Instance Name: The EC2 instance must be named `datacenter-ec2`.

AMI: Use any available Ubuntu AMI to create this instance.

User Data Script: Configure the instance to run a user data script during its launch. This script should:

Install the Nginx package.

Start the Nginx service.

Security Group: Ensure that the instance allows HTTP traffic on port  `80` from the internet.

# Solution

Below are the steps to set up the `datacenter-ec2` instance with Nginx pre-installed.

---

### Step 1: Launch the EC2 Instance

1. **Open EC2 Dashboard**: Log in to the AWS Management Console and navigate to EC2.
2. **Launch Instance**: Click **Launch instance**.
3. **Name and Tags**:
* **Name**: `datacenter-ec2`


4. **Application and OS Image (AMI)**:
* Select **Ubuntu** (e.g., Ubuntu Server 24.04 LTS).


5. **Instance Type**:
* Select **t2.micro** (or any eligible free tier type).



### Step 2: Configure the User Data Script

1. **Advanced Details**: Scroll to the bottom of the page and expand the **Advanced Details** section.
2. **User Data**: Paste the following script into the text box. This script automates the Nginx installation:
```bash
#!/bin/bash
sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx

```



### Step 3: Configure Network & Security Group

1. **Network Settings**: Click **Edit** next to Network Settings.
2. **Security Group**: Select **Create security group**.
* **Security group name**: `web-server-sg`


3. **Inbound Rules**:
* **Rule 1 (SSH)**: Port 22 (to allow you to log in later).
* **Rule 2 (HTTP)**: Add a rule for **HTTP** on **Port 80**.
* **Source**: Set to **Anywhere-IPv4 (0.0.0.0/0)**.


4. **Finalize**: Click **Launch instance**.

![1](https://github.com/user-attachments/assets/ff3276c3-8db9-4a7f-8fda-ba564e26be87)

![2](https://github.com/user-attachments/assets/db3b5d8a-e3ed-4fc5-9bfb-9a2fc5862ea8)

---

### Key Technical Considerations

* **User Data Execution**: The script runs with `root` privileges only once, during the first boot of the instance. If the script fails (e.g., due to a typo), Nginx won't start.
* **Public IP**: Ensure "Auto-assign public IP" is enabled so you can reach the server from your browser.
* **Apt Locks**: The `sudo apt update -y` command is essential to refresh the package list before attempting to install Nginx.

---

### Fun Fact

> Think of **User Data** like a "Pre-set GPS" for a rental car. Instead of getting in and figuring out where to go, the car already has the destination (Nginx) programmed and starts driving toward it the moment you turn the key!

[Dev.to](https://dev.to/hritikraj8804/aws-126-instant-web-servers-automating-nginx-with-user-data-5fd7)
