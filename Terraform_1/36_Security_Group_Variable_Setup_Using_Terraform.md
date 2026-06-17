# Instruction

The Nautilus DevOps team is enhancing infrastructure automation and needs to provision a Security Group using Terraform.

For this task:

* Create an AWS Security Group
* Security Group name: `devops-sg`
* Store the Security Group name in a variable named `KKE_sg`
* Store variables in `variables.tf`
* Reference the variable from `main.tf`
* Terraform working directory: `/home/bob/terraform`

# Solution

## Directory Structure

```text
/home/bob/terraform
├── main.tf
└── variables.tf
```

## Step 1: Create `variables.tf`

```hcl
variable "KKE_sg" {
  default = "devops-sg"
}
```

<img width="756" height="176" alt="var" src="https://github.com/user-attachments/assets/c3a9f890-c948-4bdc-ae99-87dd3f36e318" />

## Step 2: Create `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_security_group" "devops_sg" {
  name        = var.KKE_sg
  description = "Security Group created by Terraform"

  tags = {
    Name = var.KKE_sg
  }
}
```

<img width="582" height="216" alt="main" src="https://github.com/user-attachments/assets/6240f1eb-9050-4301-b151-fc4065de3423" />

### Important

Use:

```hcl
name = var.KKE_sg
```

This sets the actual Security Group name to:

```text
devops-sg
```

Many people mistakenly do only:

```hcl
tags = {
  Name = var.KKE_sg
}
```

which creates a random SG name and only sets the tag. KodeKloud validation often fails in that case.

## Step 3: Initialize Terraform

```bash
terraform init
```

## Step 4: Validate

```bash
terraform validate
```

Expected:

```text
Success! The configuration is valid.
```

## Step 5: Apply

```bash
terraform apply -auto-approve
```

## Step 6: Verify

```bash
aws ec2 describe-security-groups \
  --filters "Name=group-name,Values=devops-sg"
```

Expected:

```text
GroupName: devops-sg
```

## Challenges

### Actual Name vs Tag Name

This is the key part of the task:

```hcl
name = var.KKE_sg
```

not just:

```hcl
tags = {
  Name = var.KKE_sg
}
```

The first sets the Security Group's real AWS name, while the second only creates a tag.

## Fun Message

*"A tag gives a resource a nickname. The `name` attribute gives it an identity. Today your Security Group got both 🛡️☁️🚀"*
