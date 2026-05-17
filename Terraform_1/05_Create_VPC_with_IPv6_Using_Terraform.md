# Instruction

The Nautilus DevOps team is strategically planning the migration of a portion of their infrastructure to the AWS cloud. Acknowledging the magnitude of this endeavor, they have chosen to tackle the migration incrementally rather than as a single, massive transition. Their approach involves creating Virtual Private Clouds (VPCs) as the initial step, as they will be provisioning various services under different VPCs.

For this task:

* Create a VPC named `nautilus-vpc`
* Region must be `us-east-1`
* Use the Amazon-provided IPv6 CIDR block
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="r8m2kx"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="y4p7vd"
vi main.tf
```

OR

```bash id="u9n5qc"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="t6w1jh"
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "nautilus_vpc" {
  cidr_block                     = "10.0.0.0/16"
  assign_generated_ipv6_cidr_block = true

  tags = {
    Name = "nautilus-vpc"
  }
}
```

<img width="900" height="322" alt="main" src="https://github.com/user-attachments/assets/a4eab71d-47d3-4ed2-b581-7f5c6f51b20c" />


### **4. Initialize Terraform**

Initialize Terraform and download the required providers:

```bash id="k2v8mz"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration before deployment:

```bash id="f5x9lb"
terraform validate
```

Expected output:

```bash id="m3r7qs"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the VPC using:

```bash id="p8t4wc"
terraform apply
```

Type:

```bash id="d7n1ye"
yes
```

when prompted.



### **7. Verify the VPC**

Verify the created VPC using AWS CLI:

```bash id="z4q8ks"
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=nautilus-vpc"
```

<img width="650" height="172" alt="list" src="https://github.com/user-attachments/assets/9fecb8d0-b8a2-429a-b7a4-5ed49e52aade" />


You should see an automatically assigned IPv6 CIDR block in the output.

Or verify from the AWS Console:

* **VPC Dashboard**
* **Your VPCs**
* Search for:

```text id="j6w2hr"
nautilus-vpc
```



## Terraform Resources Used

| Resource                           | Purpose                                      |
| ---------------------------------- | -------------------------------------------- |
| `aws_vpc`                          | Creates the VPC                              |
| `assign_generated_ipv6_cidr_block` | Enables Amazon-provided IPv6 CIDR allocation |



## Challenges

### **1. Forgetting IPv6 Configuration**

Without:

```hcl id="n5u8pt"
assign_generated_ipv6_cidr_block = true
```

the VPC will only receive an IPv4 CIDR block.



### **2. Incorrect Region**

Ensure the AWS provider region is:

```hcl id="c3y7fv"
us-east-1
```

AWS resources are region-specific.



### **3. Missing Name Tag**

The task validation expects the VPC tag name to be:

```text id="b9k4mx"
nautilus-vpc
```



## Fun Message

*"Your VPC now speaks both IPv4 and IPv6 — welcome to the future of networking 🌐🚀"*
