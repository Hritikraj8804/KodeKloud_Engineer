# Instruction

The Nautilus DevOps team is currently engaged in a cleanup process, focusing on removing unnecessary resources from their AWS account. As part of this effort, an IAM role is no longer required and must be deleted.

For this task:

* Delete the IAM role named `iamrole_kirsty`
* Use Terraform
* Keep the provisioning code intact for future use
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

```bash
cd /home/bob/terraform
```



### **2. Verify the IAM Role Exists**

```bash
aws iam get-role --role-name iamrole_kirsty
```

You should see the role details if it exists.



### **3. Review the Terraform Configuration**

Open the existing `main.tf` file:

```bash
vi main.tf
```

OR

```bash
nano main.tf
```

Verify that the IAM role resource definition is still present.

Example:

```hcl
resource "aws_iam_role" "iamrole_kirsty" {
  name = "iamrole_kirsty"

  assume_role_policy = jsonencode({
    Version = "2012-10-17"
    Statement = [{
      Action = "sts:AssumeRole"
      Effect = "Allow"
      Principal = {
        Service = "ec2.amazonaws.com"
      }
    }]
  })
}
```

> Do **not** remove the code from `main.tf`. The task requires keeping the provisioning code for future use.

<img width="762" height="505" alt="config" src="https://github.com/user-attachments/assets/d1807dfa-e1bd-430f-9066-5d5ddc0b05fa" />

### **4. Initialize Terraform (If Required)**

```bash
terraform init
```



### **5. Destroy the IAM Role**

For this KodeKloud cleanup task, use the standard Terraform destroy command:

```bash
terraform destroy -auto-approve
```

Terraform will delete the IAM role while leaving the code intact in `main.tf`.

<img width="745" height="272" alt="destroy" src="https://github.com/user-attachments/assets/9171b02a-0a30-4219-8e0c-1507ea8ee307" />


### **6. Verify the IAM Role is Deleted**

Run:

```bash
aws iam get-role --role-name iamrole_kirsty
```

Expected result:

```text
NoSuchEntity
```

Or verify from the AWS Console:

* IAM Dashboard
* Roles
* Search for:

```text
iamrole_kirsty
```

The role should no longer exist.



### **7. Confirm Terraform Cleanup**

Check the Terraform state:

```bash
terraform state list
```

Expected output:

```text
No resources found
```


## Fun Message

*"Today Terraform played the role of a cloud janitor—cleaning up IAM resources while keeping the blueprint safely stored for future deployments 🧹🔐☁️🚀"*
