# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task:

* Allocate an Elastic IP address named `nautilus-eip`
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="p7x2mv"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="r4k8zn"
vi main.tf
```

OR

```bash id="w9m3qt"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="h5v1dk"
provider "aws" {
  region = "us-east-1"
}

resource "aws_eip" "nautilus_eip" {
  domain = "vpc"

  tags = {
    Name = "nautilus-eip"
  }
}
```

<img width="791" height="317" alt="main" src="https://github.com/user-attachments/assets/218fd2f9-16c3-4209-ab58-7d7e98836a5c" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash id="f8q2js"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash id="y3n7wc"
terraform validate
```

Expected output:

```bash id="u6m9xp"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Allocate the Elastic IP address using:

```bash id="c1r5tv"
terraform apply
```

Type:

```bash id="l8w4qe"
yes
```

when prompted.



### **7. Verify the Elastic IP**

Verify the Elastic IP using AWS CLI:

```bash id="n2v8ky"
aws ec2 describe-addresses
```

<img width="662" height="160" alt="res" src="https://github.com/user-attachments/assets/9f5f4402-105b-4872-94e5-1a24d7e02ab0" />


Or verify from the AWS Console:

* **EC2 Dashboard**
* **Elastic IPs**
* Search for:

```text id="m7p1dx"
nautilus-eip
```



## Terraform Resources Used

| Resource  | Purpose                                |
| --------- | -------------------------------------- |
| `aws_eip` | Allocates an Elastic IP address in AWS |



## Challenges

### **1. Missing `domain = "vpc"`**

Without:

```hcl id="z5c9rh"
domain = "vpc"
```

Terraform may use deprecated EC2-Classic behavior.



### **2. Region Mismatch**

Ensure the provider region is correctly set to:

```hcl id="t4k7jb"
us-east-1
```



### **3. Forgetting Tags**

The validation expects the Elastic IP resource name tag:

```text id="x8q3nm"
nautilus-eip
```



## Fun Message

*"You just reserved a permanent public identity in the cloud — your Elastic IP is ready for action 🌍⚡"*
