# Instruction

The Nautilus DevOps team is automating IAM role creation using Terraform to streamline permissions management.

For this task:

* Create an AWS IAM Role
* IAM Role name: `iamrole_yousuf`
* Store the IAM Role name in a variable named `KKE_iamrole`
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
variable "KKE_iamrole" {
  default = "iamrole_yousuf"
}
```

<img width="872" height="281" alt="var" src="https://github.com/user-attachments/assets/ce473632-617e-4d0d-aec5-cd144afcc39a" />

## Step 2: Create `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_role" "yousuf" {
  name = var.KKE_iamrole

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })

  tags = {
    Name = var.KKE_iamrole
  }
}
```

<img width="872" height="540" alt="main" src="https://github.com/user-attachments/assets/e145907f-a711-46b7-9a95-a91c081dfe06" />

### Important

For IAM Roles, the actual role name is set using:

```hcl
name = var.KKE_iamrole
```

and not just through tags.

The `assume_role_policy` is mandatory when creating an IAM role. A simple EC2 trust policy is sufficient unless the task specifies otherwise.

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

<img width="702" height="131" alt="apply" src="https://github.com/user-attachments/assets/e4926fb6-8b22-43bc-92ef-141bd2fffd78" />

Terraform will create:

* IAM Role: `iamrole_yousuf`

## Step 6: Verify the IAM Role

Using AWS CLI:

```bash
aws iam get-role --role-name iamrole_yousuf
```

Expected output:

```text
RoleName: iamrole_yousuf
```

Or verify from the AWS Console:

* IAM Dashboard
* Roles
* Search for:

```text
iamrole_yousuf
```

## Files Used

### `variables.tf`

```hcl
variable "KKE_iamrole" {
  default = "iamrole_yousuf"
}
```

### `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_role" "yousuf" {
  name = var.KKE_iamrole

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Principal = {
          Service = "ec2.amazonaws.com"
        }
        Action = "sts:AssumeRole"
      }
    ]
  })

  tags = {
    Name = var.KKE_iamrole
  }
}
```

## Challenges

### **1. IAM Role Requires a Trust Policy**

Unlike IAM Users and Groups, IAM Roles require an `assume_role_policy`.

Without it, Terraform will fail.

### **2. Role Name vs Tag Name**

The actual role name comes from:

```hcl
name = var.KKE_iamrole
```

The tag alone does not define the IAM role identity.

### **3. Variable Name Must Match Exactly**

Use:

```hcl
variable "KKE_iamrole"
```

exactly as specified in the task.

## Fun Message

*"Your IAM role is ready! AWS services can now assume this role and securely access resources without needing long-term credentials 🔐☁️🚀"*
