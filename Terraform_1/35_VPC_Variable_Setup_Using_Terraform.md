# Instruction

The Nautilus DevOps team is automating VPC creation using Terraform to manage networking efficiently.

For this task:

* Create an AWS VPC
* VPC Name: `devops-vpc`
* Store the VPC name in a variable named `KKE_vpc`
* CIDR Block: `10.0.0.0/16`
* Store configuration values in `variables.tf`
* Reference the variables in `main.tf`
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.


# Solution

## **Directory Structure**

```text
/home/bob/terraform
├── main.tf
└── variables.tf
```


## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

```bash
cd /home/bob/terraform
```


### **2. Create the `variables.tf` File**

```bash
vi variables.tf
```

Add:

```hcl
variable "KKE_vpc" {
  default = "devops-vpc"
}
```

<img width="752" height="370" alt="variable" src="https://github.com/user-attachments/assets/fc28a81b-c051-43df-b615-c1d96f3d1fef" />

### **3. Create the `main.tf` File**

```bash
vi main.tf
```

Add:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "devops_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = var.KKE_vpc
  }
}
```

<img width="712" height="227" alt="main" src="https://github.com/user-attachments/assets/48a3aee3-54cd-49c4-ac7c-590924113529" />

### **4. Initialize Terraform**

```bash
terraform init
```


### **5. Validate the Configuration**

```bash
terraform validate
```

Expected output:

```text
Success! The configuration is valid.
```


### **6. Apply the Configuration**

```bash
terraform apply -auto-approve
```

Terraform will create:

* VPC CIDR: `10.0.0.0/16`
* Name Tag: `devops-vpc`

<img width="552" height="146" alt="verify" src="https://github.com/user-attachments/assets/cd834bd9-c435-45f6-aaac-534fec92114f" />

### **7. Verify the VPC**

Using AWS CLI:

```bash
aws ec2 describe-vpcs \
  --filters "Name=tag:Name,Values=devops-vpc"
```

Expected output should contain:

```text
Name: devops-vpc
CidrBlock: 10.0.0.0/16
```

Or verify from the AWS Console:

* VPC Dashboard
* Your VPCs
* Search:

```text
devops-vpc
```


## Files Used

### `variables.tf`

```hcl
variable "KKE_vpc" {
  default = "devops-vpc"
}
```

### `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "devops_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = var.KKE_vpc
  }
}
```


## Challenges

### **1. Variable Name Must Match Exactly**

The task specifically requires:

```hcl
KKE_vpc
```

Using another variable name may cause validation failure.


### **2. Reference the Variable**

Use:

```hcl
var.KKE_vpc
```

instead of hardcoding:

```hcl
"devops-vpc"
```


### **3. Correct CIDR Block**

Ensure:

```hcl
cidr_block = "10.0.0.0/16"
```

matches the requirement exactly.


## Fun Message

*"You've taken the first step in building a cloud network—your VPC is now ready to host future AWS infrastructure 🌐☁️🚀"*
