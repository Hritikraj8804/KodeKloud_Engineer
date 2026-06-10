# Instruction

The Nautilus DevOps team is currently engaged in a cleanup process, focusing on removing unnecessary data and services from their AWS account. As part of the migration process, several resources were created for one-time use only, necessitating a cleanup effort to optimize their AWS environment.

For this task:

* An S3 bucket named `nautilus-bck-13109` already exists
* Copy all bucket contents to:

```text
/opt/s3-backup/
```

* Delete the S3 bucket after the backup
* Use **AWS CLI through Terraform**
* Update the existing `main.tf` file only
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

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

Add the following Terraform configuration:

```hcl
resource "null_resource" "backup_and_delete_s3" {

  provisioner "local-exec" {
    command = <<EOT
      mkdir -p /opt/s3-backup
      aws s3 cp s3://nautilus-bck-13109 /opt/s3-backup --recursive
      aws s3 rb s3://nautilus-bck-13109 --force
    EOT
  }
}
```

If your lab requires an AWS provider block, include:

```hcl
provider "aws" {
  region = "us-east-1"
}
```

<img width="957" height="302" alt="main" src="https://github.com/user-attachments/assets/620ba114-4a7c-4ddf-88d9-c2052b1d35f4" />

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



### **6. Apply the Configuration**

```bash
terraform apply -auto-approve
```

Terraform will:

1. Create `/opt/s3-backup`
2. Copy all objects from:

```text
s3://nautilus-bck-13109
```

to:

```text
/opt/s3-backup
```

3. Delete all bucket contents
4. Delete the bucket itself

<img width="761" height="281" alt="apply" src="https://github.com/user-attachments/assets/7c323a7d-4296-4381-9d06-541c84862311" />

### **7. Verify the Backup**

Verify files were copied:

```bash
ls -la /opt/s3-backup
```

You should see the objects that were previously stored in the bucket.

<img width="517" height="101" alt="verify local" src="https://github.com/user-attachments/assets/2d83fdf4-8f5f-466c-bed6-adf6fdcbc026" />

### **8. Verify Bucket Deletion**

Run:

```bash
aws s3 ls
```

Ensure:

```text
nautilus-bck-13109
```

is no longer listed.

<img width="767" height="112" alt="verify on aws" src="https://github.com/user-attachments/assets/6d96a952-38de-454e-9568-034359fc6d53" />


## Fun Message

*"Mission accomplished: data safely archived, bucket removed, and AWS cleaned up like a true DevOps engineer 🧹☁️🚀"*
