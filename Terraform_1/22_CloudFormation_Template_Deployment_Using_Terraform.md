# Instruction

The Nautilus DevOps team is working on automating infrastructure deployment using AWS CloudFormation. As part of this effort, they need to create a CloudFormation stack that provisions an S3 bucket with versioning enabled.

For this task:

* Create a CloudFormation stack named `nautilus-stack`
* The stack must create an S3 bucket named `nautilus-bucket-9665`
* Enable versioning on the S3 bucket
* Use Terraform to create the CloudFormation stack
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

resource "aws_cloudformation_stack" "nautilus_stack" {
  name = "nautilus-stack"

  template_body = <<STACK
{
  "AWSTemplateFormatVersion": "2010-09-09",
  "Resources": {
    "NautilusBucket": {
      "Type": "AWS::S3::Bucket",
      "Properties": {
        "BucketName": "nautilus-bucket-9665",
        "VersioningConfiguration": {
          "Status": "Enabled"
        }
      }
    }
  }
}
STACK
}
```

<img width="937" height="517" alt="main tf" src="https://github.com/user-attachments/assets/47172527-c316-43c2-ab46-a017fa2c7f5d" />


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

Create the CloudFormation stack using:

```bash
terraform apply -auto-approve
```

Terraform will:

* Create the CloudFormation stack `nautilus-stack`
* Deploy the S3 bucket `nautilus-bucket-9665`
* Enable versioning on the bucket



### **7. Verify the CloudFormation Stack**

Verify the stack using AWS CLI:

```bash
aws cloudformation describe-stacks \
  --stack-name nautilus-stack
```

Check that the stack status is:

```text
CREATE_COMPLETE
```



### **8. Verify the S3 Bucket and Versioning**

Verify the bucket exists:

```bash
aws s3api head-bucket --bucket nautilus-bucket-9665
```

Verify versioning:

```bash
aws s3api get-bucket-versioning \
  --bucket nautilus-bucket-9665
```

Expected output:

```json
{
  "Status": "Enabled"
}
```

<img width="596" height="177" alt="list" src="https://github.com/user-attachments/assets/5f3832ec-8a0d-4575-a595-ecb42431456e" />


## Terraform Resources Used

| Resource                   | Purpose                                         |
| -------------------------- | ----------------------------------------------- |
| `aws_cloudformation_stack` | Creates and manages an AWS CloudFormation stack |



## Challenges

### **1. Valid CloudFormation Template**

The `template_body` must contain valid CloudFormation JSON or YAML syntax.



### **2. Globally Unique Bucket Name**

S3 bucket names must be globally unique. Use the exact bucket name:

```text
nautilus-bucket-9665
```



### **3. Versioning Must Be Enabled**

Ensure the CloudFormation template contains:

```json
"VersioningConfiguration": {
  "Status": "Enabled"
}
```

Otherwise the task validation may fail.



## Fun Message

*"You just used Terraform to deploy CloudFormation to create an S3 bucket—Infrastructure as Code inside Infrastructure as Code 🚀☁️📦"*
