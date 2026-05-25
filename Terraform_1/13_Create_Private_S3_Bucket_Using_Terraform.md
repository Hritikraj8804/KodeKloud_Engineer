# Instruction

As part of the data migration process, the Nautilus DevOps team is actively creating several S3 buckets on AWS using Terraform. They plan to utilize both private and public S3 buckets to store relevant data. Since other infrastructure is also being migrated to AWS, it makes sense to centralize storage within the AWS environment.

For this task:

* Create an S3 bucket named `xfusion-s3-24754`
* The bucket must remain private
* Block all public access to the bucket
* Use Terraform
* Create resources only in the `us-east-1` region
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="x7m2qp"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="p4v9tn"
vi main.tf
```

OR

```bash id="k8x3wc"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="r5n1fj"
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "xfusion_bucket" {
  bucket = "xfusion-s3-24754"

  tags = {
    Name = "xfusion-s3-24754"
  }
}

resource "aws_s3_bucket_public_access_block" "private_bucket" {
  bucket = aws_s3_bucket.xfusion_bucket.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

<img width="1052" height="457" alt="main" src="https://github.com/user-attachments/assets/f1d28367-ba25-4780-ad04-c3a300869a97" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash id="m2q8zr"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash id="u6n4wd"
terraform validate
```

Expected output:

```bash id="f1r9ks"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the private S3 bucket using:

```bash id="y3v7tc"
terraform apply -auto-approve
```

Terraform will:

* Create the S3 bucket
* Enable block public access settings
* Ensure the bucket remains private



### **7. Verify the S3 Bucket**

Verify the bucket using AWS CLI:

```bash id="w8m5qp"
aws s3api get-public-access-block --bucket xfusion-s3-24754
```

<img width="802" height="210" alt="list" src="https://github.com/user-attachments/assets/c7fe1305-061b-4e69-afeb-24fa9a01e9e4" />


Or verify from the AWS Console:

* **S3 Dashboard**
* Open the bucket:

```text id="n4x1pd"
xfusion-s3-24754
```

* Navigate to:

  * **Permissions**
  * **Block public access**

All public access settings should be enabled.



## Terraform Resources Used

| Resource                            | Purpose                                |
| ----------------------------------- | -------------------------------------- |
| `aws_s3_bucket`                     | Creates the S3 bucket                  |
| `aws_s3_bucket_public_access_block` | Blocks all public access to the bucket |



## Challenges

### **1. Public Access Must Be Fully Blocked**

Ensure all four settings are enabled:

```hcl id="t9p3mx"
block_public_acls       = true
block_public_policy     = true
ignore_public_acls      = true
restrict_public_buckets = true
```



### **2. Bucket Names Must Be Globally Unique**

S3 bucket names are globally unique across AWS. Use the exact bucket name provided:

```text id="c7w2fd"
xfusion-s3-24754
```



### **3. Wrong Region Configuration**

Always verify the provider region:

```hcl id="v5k8rn"
region = "us-east-1"
```

AWS resources are region-specific.



## Fun Message

*"Your S3 bucket is now locked down tighter than a production database — secure cloud storage powered by Terraform 🔒☁️🚀"*
