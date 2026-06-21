# Instruction

The Nautilus DevOps team is automating IAM policy creation using Terraform to enhance security and access management.

For this task:

* Create an AWS IAM Policy
* IAM Policy name: `iampolicy_javed`
* Store the IAM Policy name in a variable named `KKE_iampolicy`
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
variable "KKE_iampolicy" {
  default = "iampolicy_javed"
}
```

<img width="777" height="222" alt="var" src="https://github.com/user-attachments/assets/2b29c89e-4d92-4808-bb80-abd636e8c7a2" />

## Step 2: Create `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_policy" "javed_policy" {
  name        = var.KKE_iampolicy
  description = "IAM policy created using Terraform"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect   = "Allow"
        Action   = [
          "ec2:Describe*"
        ]
        Resource = "*"
      }
    ]
  })
}
```

<img width="917" height="472" alt="main" src="https://github.com/user-attachments/assets/5646a4e0-9f8a-4c12-9c4f-3d5dc108b7fe" />

### Important

For IAM Policies, the actual policy name is set using:

```hcl
name = var.KKE_iampolicy
```

and not just through tags.

A valid policy document is mandatory. The simple EC2 read-only policy above is sufficient unless the task specifies a different permission set.

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

<img width="747" height="152" alt="run" src="https://github.com/user-attachments/assets/a3a52299-4f27-4fcd-aea7-860473eb1ad3" />

Terraform will create:

* IAM Policy: `iampolicy_javed`

## Step 6: Verify the IAM Policy

Using AWS CLI:

```bash
aws iam list-policies --scope Local
```

Look for:

```text
iampolicy_javed
```

Or verify from the AWS Console:

* IAM Dashboard
* Policies
* Search for:

```text
iampolicy_javed
```


## Fun Message

*"Security starts with permissions! Your IAM policy is now ready to be attached to users, groups, or roles to control AWS access 🔐☁️🚀"*
