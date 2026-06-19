# Instruction

The Nautilus DevOps team is automating IAM user creation using Terraform for better identity management.

For this task:

* Create an AWS IAM User
* IAM User name: `iamuser_kirsty`
* Store the IAM User name in a variable named `KKE_user`
* Store configuration values in `variables.tf`
* Reference the variable from `main.tf`
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.

# Solution


## Step 1: Create `variables.tf`

```hcl
variable "KKE_user" {
  default = "iamuser_kirsty"
}
```

<img width="851" height="232" alt="var" src="https://github.com/user-attachments/assets/568c7277-1168-4659-bbfc-2517c0459ed7" />

## Step 2: Create `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_user" "kirsty" {
  name = var.KKE_user

  tags = {
    Name = var.KKE_user
  }
}
```

<img width="867" height="211" alt="main" src="https://github.com/user-attachments/assets/703a3f3e-5650-46ec-895c-84fbe382a2e5" />

### Important

For IAM Users, the actual username is set using:

```hcl
name = var.KKE_user
```

not just:

```hcl
tags = {
  Name = var.KKE_user
}
```

KodeKloud typically validates the IAM username itself.

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

<img width="662" height="157" alt="apply" src="https://github.com/user-attachments/assets/8426ffc9-f90f-402a-865e-06f9dba0befc" />

Terraform will create:

* IAM User: `iamuser_kirsty`

## Step 6: Verify the IAM User

Using AWS CLI:

```bash
aws iam get-user --user-name iamuser_kirsty
```

Expected output:

```text
UserName: iamuser_kirsty
```

Or verify from the AWS Console:

* IAM Dashboard
* Users
* Search for:

```text
iamuser_kirsty
```

## Fun Message

*"Identity created successfully! Your IAM user is now ready to authenticate and interact with AWS resources securely 🔐☁️🚀"*
