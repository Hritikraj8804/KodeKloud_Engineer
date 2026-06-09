# Instruction

Data protection and recovery are fundamental aspects of data management. It's essential to have systems in place to ensure that data can be recovered in case of accidental deletion or corruption.

For this task:

* Enable versioning on the existing S3 bucket `nautilus-s3-2106`
* Use Terraform
* Update the existing `main.tf` file only
* Do not create any additional `.tf` files
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Given Existing `main.tf`**

```hcl
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-2106"
  acl    = "private"

  tags = {
    Name = "nautilus-s3-2106"
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

Add the versioning configuration to the existing bucket resource:

```hcl
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-2106"
  acl    = "private"

  versioning {
    enabled = true
  }

  tags = {
    Name = "nautilus-s3-2106"
  }
}
```

> If your lab uses a newer AWS provider version and the above gives a deprecation warning, use:

```hcl
resource "aws_s3_bucket" "s3_ran_bucket" {
  bucket = "nautilus-s3-2106"

  tags = {
    Name = "nautilus-s3-2106"
  }
}

resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.s3_ran_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}
```

<img width="920" height="502" alt="main" src="https://github.com/user-attachments/assets/ad7b71a3-2983-476b-908c-ddc820be8994" />

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

Terraform will update the existing bucket and enable versioning.

<img width="721" height="157" alt="run" src="https://github.com/user-attachments/assets/f5f9a258-6239-48ea-8128-46d7ac2bb1bb" />

### **7. Verify Versioning**

Verify using AWS CLI:

```bash
aws s3api get-bucket-versioning --bucket nautilus-s3-2106
```

Expected output:

```json
{
  "Status": "Enabled"
}
```

Or verify from the AWS Console:

* **S3**
* Open bucket:

```text
nautilus-s3-2106
```

* Navigate to:

  * **Properties**
  * **Bucket Versioning**

Status should be:

```text
Enabled
```



## Terraform Resources Used

| Resource                   | Purpose                                    |
| -------------------------- | ------------------------------------------ |
| `aws_s3_bucket`            | Manages the S3 bucket                      |
| `aws_s3_bucket_versioning` | Enables versioning (new provider versions) |



## Challenges

### **1. Provider Version Differences**

Older AWS providers support:

```hcl
versioning {
  enabled = true
}
```

Newer AWS providers use:

```hcl
aws_s3_bucket_versioning
```



### **2. Verify Versioning Status**

Before submitting, ensure:

```text
Status = Enabled
```



### **3. Keep the Existing Bucket Name**

Do not change:

```text
nautilus-s3-2106
```

The task validation expects this exact bucket.



## Fun Message

*"Versioning is your bucket's time machine—now every object change can be rolled back when needed ⏪📦☁️"*
