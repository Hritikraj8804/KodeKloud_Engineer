# Instruction

During the migration process, the Nautilus DevOps team created several EC2 instances in different AWS regions. After analyzing resource utilization, they identified an underutilized instance and decided to reduce its size to optimize costs.

For this task:

* Change the instance type of the EC2 instance `datacenter-ec2`
* Current instance type: `t2.micro`
* New instance type: `t2.nano`
* Use Terraform
* Ensure the instance is in the **Running** state after the change
* Wait for the EC2 status checks to complete if they are still in the **Initializing** state
* Terraform working directory: `/home/bob/terraform`
* Update the existing `main.tf` file only

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.

# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash
cd /home/bob/terraform
```

### **2. Verify Instance Status**

Before making any changes, ensure the instance is fully initialized:

```bash
aws ec2 describe-instance-status
```

Wait until:

```text
Instance Status  : passed
System Status    : passed
```

If the instance is still showing:

```text
Initializing
```

wait a few minutes and check again.

<img width="715" height="541" alt="beforestatus" src="https://github.com/user-attachments/assets/f79bfa54-704e-4c92-ae31-c184540e3ef1" />

### **3. Open the Existing `main.tf` File**

Edit the existing Terraform configuration:

```bash
vi main.tf
```

OR

```bash
nano main.tf
```

### **4. Update the Instance Type**

Locate the EC2 resource for `datacenter-ec2`.

You will likely find:

```hcl
resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-xxxxxxxx"
  instance_type = "t2.micro"

  tags = {
    Name = "datacenter-ec2"
  }
}
```

Update only the instance type:

```hcl
resource "aws_instance" "datacenter_ec2" {
  ami           = "ami-xxxxxxxx"
  instance_type = "t2.nano"

  tags = {
    Name = "datacenter-ec2"
  }
}
```

<img width="767" height="355" alt="after" src="https://github.com/user-attachments/assets/7319e3c4-9805-464d-bcb8-56aa82f370aa" />

### **5. Initialize Terraform (If Needed)**

If the directory has not been initialized:

```bash
terraform init
```

### **6. Validate the Configuration**

Validate the Terraform configuration:

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```

### **7. Apply the Changes**

Apply the Terraform changes:

```bash
terraform apply -auto-approve
```

Terraform will:

* Stop the EC2 instance (if required)
* Modify the instance type
* Restart the instance
* Update the Terraform state

<img width="810" height="532" alt="apply" src="https://github.com/user-attachments/assets/5b9a147b-07a5-4b61-895b-c174535c50c2" />

### **8. Verify the Instance Type**

Verify the updated instance type:

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=datacenter-ec2"
```

Look for:

```text
InstanceType : t2.nano
```

### **9. Verify the Instance State**

Ensure the instance is running:

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=datacenter-ec2" \
  --query "Reservations[*].Instances[*].State.Name"
```

Expected output:

```text
running
```

### **10. Confirm Terraform State**

Run:

```bash
terraform plan
```

Expected output:

```text
No changes. Your infrastructure matches the configuration.
```

<img width="777" height="242" alt="verify" src="https://github.com/user-attachments/assets/fbf28a87-7540-420f-9254-e6ffda25fa8d" />

## Terraform Resources Used

| Resource       | Purpose                  |
| -------------- | ------------------------ |
| `aws_instance` | Manages the EC2 instance |


## Fun Message

*"A smaller instance, lower costs, same infrastructure—Terraform just helped optimize your cloud bill 💰☁️🚀"*
