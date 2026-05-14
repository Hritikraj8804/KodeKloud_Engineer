# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Use Terraform to create a security group under the default VPC with the following requirements:

* The name of the security group must be `nautilus-sg`.
* The description must be `Security group for Nautilus App Servers`.
* Add an inbound rule of type `HTTP`, with port `80`, and source CIDR range `0.0.0.0/0`.
* Add another inbound rule of type `SSH`, with port `22`, and source CIDR range `0.0.0.0/0`.
* Ensure the security group is created in the `us-east-1` region using Terraform.
* The Terraform working directory is `/home/bob/terraform`.
* Create the `main.tf` file only.

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="g6e7x2"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="u8s4mn"
vi main.tf
```

OR

```bash id="m2k7qp"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="q9v1tj"
provider "aws" {
  region = "us-east-1"
}

data "aws_vpc" "default" {
  default = true
}

resource "aws_security_group" "nautilus_sg" {
  name        = "nautilus-sg"
  description = "Security group for Nautilus App Servers"
  vpc_id      = data.aws_vpc.default.id

  ingress {
    description = "HTTP Access"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "SSH Access"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

<img width="937" height="720" alt="main" src="https://github.com/user-attachments/assets/d87bf040-962a-4344-9644-606412942903" />


### **4. Initialize Terraform**

Initialize Terraform and download the AWS provider:

```bash id="w7c2ke"
terraform init
```



### **5. Validate the Configuration**

Validate the Terraform configuration:

```bash id="b5r9hf"
terraform validate
```

Expected output:

```bash id="n3q8yt"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Run the following command to create the security group:

```bash id="v1s6xd"
terraform apply
```

Type:

```bash id="r4j2wm"
yes
```

when prompted.


<img width="715" height="205" alt="r" src="https://github.com/user-attachments/assets/945175fb-04cf-4038-8ada-ab0587d961a8" />

### **7. Verify the Security Group**

You can verify the created security group using:

```bash id="z7h4op"
aws ec2 describe-security-groups --group-names nautilus-sg
```

Or check from the AWS Console:

* **EC2 Dashboard**
* **Security Groups**
* Search for:

```text id="d9m5la"
nautilus-sg
```



## Terraform Resources Used

| Resource              | Purpose                    |
| --------------------- | -------------------------- |
| `aws_security_group`  | Creates the security group |
| `aws_vpc` data source | Fetches the default VPC    |



## Challenges

### **1. Using the Wrong VPC**

If the security group is not attached to the default VPC, the validation may fail.



### **2. Incorrect Port Configuration**

Ensure:

* HTTP uses port `80`
* SSH uses port `22`



### **3. Region Mismatch**

Always verify the provider region is:

```hcl id="f3w8uy"
region = "us-east-1"
```

AWS resources are region-specific.



## Fun Message

*"Your servers are now protected by Terraform-managed cloud shields 🛡️☁️"*
