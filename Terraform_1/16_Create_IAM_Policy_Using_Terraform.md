# Instruction

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls.

For this task:

* Create an IAM policy named `iampolicy_javed`
* The policy must provide **read-only access to Amazon EC2**
* Users should be able to view:

  * EC2 Instances
  * AMIs
  * Snapshots
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

resource "aws_iam_policy" "iampolicy_javed" {
  name        = "iampolicy_javed"
  description = "Read-only access to EC2 resources"

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Effect = "Allow"
        Action = [
          "ec2:DescribeInstances",
          "ec2:DescribeImages",
          "ec2:DescribeSnapshots",
          "ec2:DescribeVolumes",
          "ec2:DescribeSecurityGroups",
          "ec2:DescribeVpcs",
          "ec2:DescribeSubnets",
          "ec2:DescribeKeyPairs",
          "ec2:DescribeTags"
        ]
        Resource = "*"
      }
    ]
  })
}
```

<img width="842" height="445" alt="main" src="https://github.com/user-attachments/assets/1086d3b9-85ae-4a92-9829-9908e3fcfe71" />


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

Create the IAM policy using:

```bash
terraform apply -auto-approve
```

Terraform will:

* Create the IAM policy
* Store the policy ARN in the Terraform state
* Make the policy available for attachment to users, groups, or roles



### **7. Verify the IAM Policy**

Verify the policy using AWS CLI:

```bash
aws iam list-policies --scope Local
```

Look for:

```text
iampolicy_javed
```

Or verify from the AWS Console:

* **IAM Dashboard**
* **Policies**
* Search for:

```text
iampolicy_javed
```

<img width="695" height="212" alt="list" src="https://github.com/user-attachments/assets/38d5c4df-b7eb-4520-b058-eaae3b4deedc" />


## Terraform Resources Used

| Resource         | Purpose                     |
| ---------------- | --------------------------- |
| `aws_iam_policy` | Creates a custom IAM policy |



## Challenges

### **1. IAM is a Global Service**

IAM policies are global AWS resources and are not tied to a specific region.



### **2. Read-Only Permissions Only**

The policy should only contain `Describe*` actions and must not include any actions that modify resources.



### **3. Correct Policy Name**

Ensure the IAM policy name is exactly:

```text
iampolicy_javed
```

Otherwise the task validation may fail.



## Fun Message

*"You've created a security guard for AWS—one that can look at everything in EC2 but can't touch a thing 👀🔐☁️"*
