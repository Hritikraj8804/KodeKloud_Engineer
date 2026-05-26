# Instruction

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls.

For this task:

* Create an IAM user named `iamuser_jim`
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash
vi main.tf
```

OR

```bash
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_user" "iamuser_jim" {
  name = "iamuser_jim"

  tags = {
    Name = "iamuser_jim"
  }
}
```

<img width="861" height="188" alt="main" src="https://github.com/user-attachments/assets/833dde52-103d-4542-b438-42294f502eb0" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the IAM user using:

```bash
terraform apply -auto-approve
```

Terraform will:

* Connect to AWS
* Create the IAM user
* Store the resource state locally

<img width="733" height="530" alt="apply" src="https://github.com/user-attachments/assets/b6875a36-22e4-4b77-a2e9-6bc718fdd615" />


### **7. Verify the IAM User**

Verify the IAM user using AWS CLI:

```bash
aws iam get-user --user-name iamuser_jim
```

Or verify from the AWS Console:

* **IAM Dashboard**
* **Users**
* Search for:

```text
iamuser_jim
```

<img width="638" height="67" alt="list" src="https://github.com/user-attachments/assets/16e54422-cd47-413a-8b36-6a530340da52" />


## Terraform Resources Used

| Resource       | Purpose                    |
| -------------- | -------------------------- |
| `aws_iam_user` | Creates an IAM user in AWS |



## Challenges

### **1. IAM is a Global Service**

Unlike EC2 or VPC resources, IAM resources are global and are not tied to a specific AWS region.



### **2. User Name Must Match Exactly**

Ensure the IAM user name is:

```text
iamuser_jim
```

Any typo may cause the validation to fail.



### **3. AWS Credentials**

Terraform must have valid AWS credentials configured before running:

```bash
terraform apply
```

Otherwise resource creation will fail.



## Fun Message

*"Congratulations! You've created your first cloud identity. Every AWS journey starts with the right permissions 🔐☁️🚀"*
