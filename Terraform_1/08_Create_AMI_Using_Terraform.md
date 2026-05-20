# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task:

* Create an AMI from an existing EC2 instance named `xfusion-ec2`
* The AMI name should be `xfusion-ec2-ami`
* Ensure the AMI reaches the `available` state
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Update the existing `main.tf` file only

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="f8m2qy"
cd /home/bob/terraform
```



### **2. Open the Existing `main.tf` File**

Edit the existing Terraform configuration file:

```bash id="p4x7vn"
vi main.tf
```

OR

```bash id="u9k3dc"
nano main.tf
```



### **3. Update the Terraform Configuration**

Replace the existing content with the following configuration:

```hcl id="w6r1jt"
provider "aws" {
  region = "us-east-1"
}

# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"

  vpc_security_group_ids = [
    "sg-6da895c0eac4d67a5"
  ]

  tags = {
    Name = "xfusion-ec2"
  }
}

# Create AMI from existing EC2 instance
resource "aws_ami_from_instance" "xfusion_ami" {
  name               = "xfusion-ec2-ami"
  source_instance_id = aws_instance.ec2.id

  depends_on = [
    aws_instance.ec2
  ]
}
```

<img width="876" height="631" alt="main" src="https://github.com/user-attachments/assets/2b8c3f9a-3364-419b-9787-bdbfeeea379f" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash id="k5n8zx"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the configuration before deployment:

```bash id="r2v6pd"
terraform validate
```

Expected output:

```bash id="y7m1qs"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Run the following command to create the EC2 instance and AMI:

```bash id="d4x9lt"
terraform apply -auto-approve
```

Terraform will:

* Launch the EC2 instance
* Create the AMI from the instance
* Wait until the AMI becomes available



### **7. Verify the AMI**

Verify the created AMI using AWS CLI:

```bash id="m8q3wr"
aws ec2 describe-images --owners self
```

Look for the AMI name:

```text id="v1p7kc"
xfusion-ec2-ami
```

and ensure the state shows:

```text id="c6r2yn"
available
```

You can also verify from the AWS Console:

* **EC2 Dashboard**
* **AMIs**
* Search for:

```text id="z9w5mf"
xfusion-ec2-ami
```

<img width="697" height="167" alt="list" src="https://github.com/user-attachments/assets/d8f2b991-40d6-45c8-ba50-6b2a9592cf6d" />


## Terraform Resources Used

| Resource                | Purpose                              |
| ----------------------- | ------------------------------------ |
| `aws_instance`          | Creates the EC2 instance             |
| `aws_ami_from_instance` | Creates an AMI from the EC2 instance |



## Challenges

### **1. AMI Creation Takes Time**

Even after Terraform finishes, AWS may take a few moments to change the AMI state to:

```text id="h4n8qd"
available
```



### **2. Wrong Source Instance**

Ensure the AMI is created from the correct EC2 instance:

```text id="s7x1vp"
xfusion-ec2
```



### **3. Security Group Issues**

The EC2 instance uses the existing security group:

```text id="j2m9lc"
sg-6da895c0eac4d67a5
```

Make sure the security group exists before applying Terraform.



## Fun Message

*"Terraform just cloned your server into a reusable machine image cloud automation at its finest 📸☁️🚀"*
