# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. As part of this phased migration approach, they need to allocate an Elastic IP address to support external access for specific workloads.

For this task:

* Create an AWS Elastic IP
* Store the Elastic IP name `datacenter-eip` in a variable named `KKE_eip`
* Store configuration values in `variables.tf`
* Reference the variable from `main.tf`
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.

# Solution

## Directory Structure

```text
/home/bob/terraform
├── main.tf
└── variables.tf
```

## Step 1: Create `variables.tf`

```hcl
variable "KKE_eip" {
  default = "datacenter-eip"
}
```

<img width="657" height="202" alt="vari" src="https://github.com/user-attachments/assets/04d55265-ea57-457b-9920-52e88f7cea82" />

## Step 2: Create `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_eip" "datacenter_eip" {
  domain = "vpc"

  tags = {
    Name = var.KKE_eip
  }
}
```

<img width="722" height="227" alt="main" src="https://github.com/user-attachments/assets/9b1d3842-9810-4e1f-beaf-44904e2b6181" />

> Unlike Security Groups, Elastic IPs do not have a `name` attribute. The name is stored as a **Name tag**, which is what AWS displays and what KodeKloud typically validates for EIP tasks.

## Step 3: Initialize Terraform

```bash
terraform init
```

## Step 4: Validate the Configuration

```bash
terraform validate
```

Expected output:

```text
Success! The configuration is valid.
```

## Step 5: Apply the Configuration

```bash
terraform apply -auto-approve
```

<img width="532" height="112" alt="apply" src="https://github.com/user-attachments/assets/469acfa1-8ef4-4b07-830b-9df7154321c8" />

Terraform will:

* Allocate a new Elastic IP
* Assign the Name tag `datacenter-eip`

## Step 6: Verify the Elastic IP

Using AWS CLI:

```bash
aws ec2 describe-addresses \
  --filters "Name=tag:Name,Values=datacenter-eip"
```

Expected output should contain:

```text
Name: datacenter-eip
```

Or verify from the AWS Console:

* EC2 Dashboard
* Elastic IPs
* Search for:

```text
datacenter-eip
```

## Files Used

### `variables.tf`

```hcl
variable "KKE_eip" {
  default = "datacenter-eip"
}
```

### `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_eip" "datacenter_eip" {
  domain = "vpc"

  tags = {
    Name = var.KKE_eip
  }
}
```

## Challenges

### **1. Elastic IP Has No Name Attribute**

Unlike Security Groups:

```hcl
name = var.KKE_sg
```

Elastic IPs do not support a `name` argument.

The correct approach is:

```hcl
tags = {
  Name = var.KKE_eip
}
```

### **2. Variable Name Must Match Exactly**

Use:

```hcl
variable "KKE_eip"
```

as specified in the task.

### **3. Reference the Variable**

Use:

```hcl
var.KKE_eip
```

instead of hardcoding the value.

## Fun Message

*"Your Elastic IP is now reserved and waiting for a workload to call home—one more building block in your AWS migration journey 🌍☁️🚀"*
