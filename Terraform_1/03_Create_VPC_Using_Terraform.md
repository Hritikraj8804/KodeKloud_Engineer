# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task:

* Create a VPC named `datacenter-vpc`
* Use Terraform
* Region must be `us-east-1`
* Use any valid IPv4 CIDR block
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="j8n2xq"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="k5v9tm"
vi main.tf
```

OR

```bash id="q7m3ws"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="h2r8df"
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "datacenter_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "datacenter-vpc"
  }
}
```

<img width="795" height="257" alt="main" src="https://github.com/user-attachments/assets/cfa02abe-ff64-499c-a34d-56ede192e39a" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash id="y4t8pz"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the configuration before applying:

```bash id="d6w1ku"
terraform validate
```

Expected output:

```bash id="e9r5mn"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the VPC using:

```bash id="u3x7fa"
terraform apply
```

Type:

```bash id="c2b9lh"
yes
```

when prompted.

<img width="595" height="167" alt="list" src="https://github.com/user-attachments/assets/006d3064-767d-430d-8788-a61730dd6f3a" />


### **7. Verify the VPC**

Verify the created VPC using AWS CLI:

```bash id="n7k4sv"
aws ec2 describe-vpcs --filters "Name=tag:Name,Values=datacenter-vpc"
```

Or verify from the AWS Console:

* **VPC Dashboard**
* **Your VPCs**
* Search for:

```text id="z5p1dx"
datacenter-vpc
```



## Terraform Resources Used

| Resource  | Purpose                             |
| --------- | ----------------------------------- |
| `aws_vpc` | Creates a new Virtual Private Cloud |



## Challenges

### **1. Invalid CIDR Block**

Ensure the CIDR block is valid IPv4 format, for example:

```hcl id="m8w2rq"
10.0.0.0/16
```



### **2. Wrong Region**

The provider region must be:

```hcl id="v4j7ce"
us-east-1
```

Otherwise, the VPC may be created in the wrong AWS region.



### **3. Forgetting Tags**

Without the correct `Name` tag, the VPC may not match the required validation.



## Fun Message

*"You just built the foundation of your cloud network — one VPC closer to becoming a Terraform wizard ☁️🚀"*
