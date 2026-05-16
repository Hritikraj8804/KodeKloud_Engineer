# Instruction

The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

For this task:

* Create a VPC named `xfusion-vpc`
* Region must be `us-east-1`
* Use IPv4 CIDR block `192.168.0.0/24`
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="t7m2qx"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="g5v9ln"
vi main.tf
```

OR

```bash id="u4k8dc"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="x2r7pf"
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "xfusion_vpc" {
  cidr_block = "192.168.0.0/24"

  tags = {
    Name = "xfusion-vpc"
  }
}
```



### **4. Initialize Terraform**

Initialize Terraform and download the AWS provider:

```bash id="y6p1zs"
terraform init
```



### **5. Validate the Configuration**

Validate the Terraform configuration before deployment:

```bash id="n8c4wd"
terraform validate
```

Expected output:

```bash id="f3h9rq"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the VPC using:

```bash id="m2v7ka"
terraform apply
```

Type:

```bash id="d5x8ju"
yes
```

when prompted.



### **7. Verify the VPC**

Verify the created VPC using AWS CLI:

```bash id="q9l3te"
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=xfusion-vpc"
```

Or check from the AWS Console:

* **VPC Dashboard**
* **Your VPCs**
* Search for:

```text id="h7r2mc"
xfusion-vpc
```

<img width="965" height="682" alt="main" src="https://github.com/user-attachments/assets/9c8d8d2a-44d1-4ab0-b0f3-15bd7c5aad1d" />


## Terraform Resources Used

| Resource  | Purpose                  |
| --------- | ------------------------ |
| `aws_vpc` | Creates a new VPC in AWS |



## Challenges

### **1. CIDR Block Accuracy**

Ensure the exact CIDR block is used:

```hcl id="p4w8nb"
192.168.0.0/24
```

Using a different subnet range may fail validation.



### **2. Correct Region**

Always verify the provider region is:

```hcl id="k6s1yo"
us-east-1
```

AWS resources are region-specific.



### **3. Missing Name Tag**

The validation expects the VPC name tag to be:

```text id="e8d5vp"
xfusion-vpc
```

Without the correct tag, the task may fail.



## Fun Message

*"Every cloud journey starts with a network — and today, Terraform built yours automatically ☁️⚡"*
