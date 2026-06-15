# Instruction

The Nautilus DevOps team is currently migrating data from on-premise storage to AWS S3. They have received a file that needs to be uploaded to an existing S3 bucket.

For this task:

* S3 bucket `nautilus-cp-9674` already exists
* Copy the file `/tmp/nautilus.txt`
* Upload it to the S3 bucket `nautilus-cp-9674`
* Use Terraform
* Update the existing `main.tf` file only
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.


# Solution

## **Given Existing `main.tf`**

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "nautilus-cp-9674"
  acl    = "private"

  tags = {
    Name = "nautilus-cp-9674"
  }
}
```


## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

```bash
cd /home/bob/terraform
```


### **2. Open the Existing `main.tf` File**

```bash
vi main.tf
```

OR

```bash
nano main.tf
```


### **3. Update the Terraform Configuration**

Add an S3 object resource to upload the file:

```hcl
resource "aws_s3_bucket" "my_bucket" {
  bucket = "nautilus-cp-9674"
  acl    = "private"

  tags = {
    Name = "nautilus-cp-9674"
  }
}

resource "aws_s3_object" "nautilus_file" {
  bucket = aws_s3_bucket.my_bucket.id
  key    = "nautilus.txt"
  source = "/tmp/nautilus.txt"
}
```

> Some AWS provider versions may use `aws_s3_bucket_object` instead of `aws_s3_object`:

```hcl
resource "aws_s3_bucket_object" "nautilus_file" {
  bucket = aws_s3_bucket.my_bucket.id
  key    = "nautilus.txt"
  source = "/tmp/nautilus.txt"
}
```

<img width="860" height="472" alt="main" src="https://github.com/user-attachments/assets/8784e989-bfb5-4a6c-b5e3-675467fd9835" />

### **4. Initialize Terraform**

```bash
terraform init
```


### **5. Validate the Configuration**

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```


### **6. Apply the Changes**

```bash
terraform apply -auto-approve
```

Terraform will:

* Use the existing bucket
* Upload `/tmp/nautilus.txt`
* Create the object `nautilus.txt` inside the bucket

<img width="702" height="342" alt="apply" src="https://github.com/user-attachments/assets/630eab7e-5242-46f1-a338-f6d2402501d3" />

### **7. Verify the File Upload**

Verify using AWS CLI:

```bash
aws s3 ls s3://nautilus-cp-9674
```

Expected output:

```text
nautilus.txt
```

Or verify from the AWS Console:

* S3
* Open bucket:

```text
nautilus-cp-9674
```

You should see:

```text
nautilus.txt
```

inside the bucket.


## Terraform Resources Used

| Resource        | Purpose                  |
| --------------- | ------------------------ |
| `aws_s3_bucket` | References the S3 bucket |
| `aws_s3_object` | Uploads a file to S3     |


## Fun Message

*"Your file has successfully completed its cloud migration journey—from local storage to S3 with Terraform as the transport service 📦☁️🚀"*
