# Instruction

As part of the data migration process, the Nautilus DevOps team is actively creating several S3 buckets on AWS. They plan to utilize both private and public S3 buckets to store relevant data. Since other infrastructure components are also being migrated to AWS, it makes sense to centralize data storage within the AWS ecosystem.

For this task:

* Create a public S3 bucket named `datacenter-s3-27707`
* Ensure the bucket is publicly accessible
* Configure the proper ACL for public access
* Use Terraform
* Create resources only in the `us-east-1` region
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="k8m2qx"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="p5v9tn"
vi main.tf
```

OR

```bash id="r2x7wc"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="t6n1fj"
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "datacenter_bucket" {
  bucket = "datacenter-s3-27707"

  tags = {
    Name = "datacenter-s3-27707"
  }
}

resource "aws_s3_bucket_acl" "bucket_acl" {
  bucket = aws_s3_bucket.datacenter_bucket.id
  acl    = "public-read"
}

resource "aws_s3_bucket_public_access_block" "public_access" {
  bucket = aws_s3_bucket.datacenter_bucket.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}
```

<img width="953" height="540" alt="main" src="https://github.com/user-attachments/assets/c1d68d35-9fd7-45ce-bb41-824a54cc581f" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash id="m4q8zp"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash id="x7n3wd"
terraform validate
```

Expected output:

```bash id="f9r2ks"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the public S3 bucket using:

```bash id="u1v6tc"
terraform apply -auto-approve
```

Terraform will:

* Create the S3 bucket
* Configure public ACL permissions
* Disable public access restrictions

<img width="762" height="202" alt="apply" src="https://github.com/user-attachments/assets/542d22fd-7737-43bd-9b8c-96c911212126" />


### **7. Verify the S3 Bucket**

Verify the bucket using AWS CLI:

```bash id="d5m9qy"
aws s3api get-bucket-acl --bucket datacenter-s3-27707
```

Or verify from the AWS Console:

* **S3 Dashboard**
* Open the bucket:

```text id="w3k7pn"
datacenter-s3-27707
```

* Check:

  * **Permissions**
  * **Public access**
  * **Bucket ACL**

The bucket should show public access enabled.

<img width="566" height="101" alt="lis" src="https://github.com/user-attachments/assets/0eca768d-2db1-44c2-a701-9d799aa1c3cf" />


## Terraform Resources Used

| Resource                            | Purpose                       |
| ----------------------------------- | ----------------------------- |
| `aws_s3_bucket`                     | Creates the S3 bucket         |
| `aws_s3_bucket_acl`                 | Sets bucket ACL permissions   |
| `aws_s3_bucket_public_access_block` | Allows public access settings |



## Challenges

### **1. Public Access Block Restrictions**

AWS blocks public bucket access by default. Ensure these settings are disabled:

```hcl id="y8p4mx"
block_public_acls       = false
block_public_policy     = false
ignore_public_acls      = false
restrict_public_buckets = false
```



### **2. Correct ACL Setting**

The task specifically requires a public bucket, so ensure:

```hcl id="n6v1rt"
acl = "public-read"
```

is configured correctly.



### **3. Globally Unique Bucket Names**

S3 bucket names must be globally unique across AWS. The task already provides:

```text id="c2w9fd"
datacenter-s3-27707
```

which should be used exactly.



## Fun Message

*"Your S3 bucket is now live and publicly accessible — Terraform just turned AWS into your cloud storage powerhouse ☁️📦🚀"*
