# Instruction

The James DevOps team has been creating several services on AWS Cloud. To simplify the migration process and reduce operational risks, they have divided the migration into smaller and more manageable tasks.

For this task:

* Create an IAM group named `iamgroup_james`
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

resource "aws_iam_group" "iamgroup_james" {
  name = "iamgroup_james"
}
```

<img width="878" height="265" alt="main" src="https://github.com/user-attachments/assets/92ee539a-6e3e-4a45-ae91-45fe472a01d5" />


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

Create the IAM group using:

```bash
terraform apply -auto-approve
```

Terraform will:

* Connect to AWS
* Create the IAM group
* Save the resource information in the Terraform state file



### **7. Verify the IAM Group**

Verify the IAM group using AWS CLI:

```bash
aws iam get-group --group-name iamgroup_james
```

Or verify from the AWS Console:

* **IAM Dashboard**
* **User Groups**
* Search for:

```text
iamgroup_james
```

<img width="707" height="183" alt="list" src="https://github.com/user-attachments/assets/d144312c-ae35-41ee-acee-7c349435af43" />


## Terraform Resources Used

| Resource        | Purpose                     |
| --------------- | --------------------------- |
| `aws_iam_group` | Creates an IAM group in AWS |



## Challenges

### **1. IAM is a Global AWS Service**

IAM groups are global resources and are not tied to any specific AWS region.



### **2. Group Name Must Match Exactly**

Ensure the IAM group name is:

```text
iamgroup_james
```

Any typo may cause the task validation to fail.



### **3. AWS Credentials Must Be Configured**

Terraform requires valid AWS credentials before running:

```bash
terraform apply
```

Otherwise, resource creation will fail.



## Fun Message

*"You've just created a cloud team! IAM groups make managing permissions for multiple users much easier 👥☁️🚀"*
