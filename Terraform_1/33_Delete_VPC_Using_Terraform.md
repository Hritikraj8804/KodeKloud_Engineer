# Instruction

The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud. As part of ongoing cleanup efforts, some resources are no longer needed and can be safely removed.

For this task:

* Delete the VPC named `nautilus-vpc`
* Region: `us-east-1`
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


### **2. Verify the VPC Exists**

```bash
aws ec2 describe-vpcs \
  --filters "Name=tag:Name,Values=nautilus-vpc" \
  --region us-east-1
```

Confirm that the VPC is present before proceeding.

### **3. Review the Terraform Configuration**

Open the existing `main.tf` file:

```bash
vi main.tf
```

OR

```bash
nano main.tf
```

Verify that the VPC resource definition remains in the file.

Example:

```hcl
resource "aws_vpc" "nautilus_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "nautilus-vpc"
  }
}
```

> Do **not** delete the Terraform code. The task explicitly states that the provisioning code should be retained.

<img width="687" height="260" alt="config" src="https://github.com/user-attachments/assets/dd7c17e5-2dcd-4677-ba40-eab8c6b39c94" />

### **4. Initialize Terraform (If Required)**

```bash
terraform init
```


### **5. Destroy the VPC**

For KodeKloud cleanup tasks, use the standard Terraform destroy command:

```bash
terraform destroy -auto-approve
```

Terraform will delete the VPC and remove it from the state file while keeping the configuration in `main.tf`.

<img width="766" height="231" alt="destroy" src="https://github.com/user-attachments/assets/78f88450-afc6-4dac-9eb9-65faacf5dff8" />

### **6. Verify the VPC is Deleted**

Run:

```bash
aws ec2 describe-vpcs \
  --filters "Name=tag:Name,Values=nautilus-vpc" \
  --region us-east-1
```

The VPC should no longer appear in the output.

You can also verify from the AWS Console:

* VPC Dashboard
* Your VPCs
* Search for:

```text
nautilus-vpc
```

The VPC should no longer exist.


### **7. Confirm Terraform Cleanup**

Check the Terraform state:

```bash
terraform state list
```

Expected output:

```text
No resources found
```

or an empty state depending on the lab setup.

<img width="540" height="52" alt="list after" src="https://github.com/user-attachments/assets/9c4dc988-42b1-4952-95de-861265580b0f" />

## Terraform Commands Used

| Command                           | Purpose                                         |
| --------------------------------- | ----------------------------------------------- |
| `terraform init`                  | Initializes Terraform                           |
| `terraform destroy -auto-approve` | Deletes the deployed VPC while keeping the code |


## Fun Message

*"Infrastructure as Code isn't just for creating cloud resources—today Terraform helped retire a VPC while preserving the blueprint for future deployments 🧹☁️🚀"*
