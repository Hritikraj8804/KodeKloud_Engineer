# Instruction

The Nautilus DevOps team has been creating several services on AWS Cloud and is optimizing resource utilization as part of their migration process.

For this task:

* Attach the existing Elastic IP `datacenter-ec2-eip`
* To the existing EC2 instance `datacenter-ec2`
* Use Terraform only
* Update the existing `main.tf` file
* Do not create any additional `.tf` files
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Given Existing `main.tf`**

```hcl
# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-d6bc755547ae0aff8"

  vpc_security_group_ids = [
    "sg-741feabc2a190ebe6"
  ]

  tags = {
    Name = "datacenter-ec2"
  }
}

# Provision Elastic IP
resource "aws_eip" "ec2_eip" {
  tags = {
    Name = "datacenter-ec2-eip"
  }
}
```

<img width="855" height="652" alt="config" src="https://github.com/user-attachments/assets/b02c34c3-eebd-42d9-8f47-2efecdef3012" />

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

```bash
cd /home/bob/terraform
```



### **2. Open the Existing `main.tf` File**

```bash
vi main.tf
```

OR

```bash
nano main.tf
```



### **3. Update the Terraform Configuration**

Add the Elastic IP association by updating the EIP resource as shown below:

```hcl
provider "aws" {
  region = "us-east-1"
}

# Provision EC2 instance
resource "aws_instance" "ec2" {
  ami           = "ami-0c101f26f147fa7fd"
  instance_type = "t2.micro"
  subnet_id     = "subnet-d6bc755547ae0aff8"

  vpc_security_group_ids = [
    "sg-741feabc2a190ebe6"
  ]

  tags = {
    Name = "datacenter-ec2"
  }
}

# Provision and Attach Elastic IP
resource "aws_eip" "ec2_eip" {
  instance = aws_instance.ec2.id

  tags = {
    Name = "datacenter-ec2-eip"
  }
}
```



### **4. Initialize Terraform**

```bash
terraform init
```



### **5. Validate the Configuration**

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```



### **6. Apply the Changes**

```bash
terraform apply -auto-approve
```

Terraform will:

* Create/update the EC2 instance
* Allocate the Elastic IP
* Automatically associate the Elastic IP with the instance

<img width="680" height="102" alt="result" src="https://github.com/user-attachments/assets/d8713cac-97f9-44e4-9aa2-78b0d230f1dd" />

### **7. Verify the Elastic IP Association**

Check the EIP:

```bash
aws ec2 describe-addresses
```

You should see:

```text
InstanceId: i-xxxxxxxxxxxx
PublicIp: <Elastic-IP>
```

Verify the instance:

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=datacenter-ec2"
```

The instance should show the Elastic IP as its public IP.



## Terraform Resources Used

| Resource       | Purpose                                |
| -------------- | -------------------------------------- |
| `aws_instance` | Creates the EC2 instance               |
| `aws_eip`      | Allocates and associates an Elastic IP |


## Fun Message

*"Your EC2 instance now has a permanent public identity—Elastic IP attached and ready for the internet 🌐⚡☁️"*
