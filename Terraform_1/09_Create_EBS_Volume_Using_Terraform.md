# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task:

* Create an AWS EBS volume named `devops-volume`
* Volume type must be `gp3`
* Volume size must be `2 GiB`
* Ensure the volume is created in `us-east-1`
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="p4x8mv"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="k7n2qd"
vi main.tf
```

OR

```bash id="m5v9rc"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="t8w3fj"
provider "aws" {
  region = "us-east-1"
}

resource "aws_ebs_volume" "devops_volume" {
  availability_zone = "us-east-1a"
  size              = 2
  type              = "gp3"

  tags = {
    Name = "devops-volume"
  }
}
```

<img width="805" height="310" alt="main" src="https://github.com/user-attachments/assets/90cb95a9-2182-4a24-b39b-0d7885a16e79" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash id="y2r7lx"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash id="f9k4wp"
terraform validate
```

Expected output:

```bash id="u6m1qs"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the EBS volume using:

```bash id="d3v8tn"
terraform apply
```

Type:

```bash id="n5x2ke"
yes
```

when prompted.



### **7. Verify the EBS Volume**

Verify the created EBS volume using AWS CLI:

```bash id="w1q9rc"
aws ec2 describe-volumes
```

Look for the volume tag:

```text id="z4m7pd"
devops-volume
```

Or verify from the AWS Console:

* **EC2 Dashboard**
* **Elastic Block Store**
* **Volumes**
* Search for:

```text id="j8t3vn"
devops-volume
```

<img width="607" height="170" alt="list" src="https://github.com/user-attachments/assets/d5ed3e75-8fac-470d-8047-c4f10558f109" />


## Terraform Resources Used

| Resource         | Purpose                       |
| ---------------- | ----------------------------- |
| `aws_ebs_volume` | Creates an EBS storage volume |



## Challenges

### **1. Availability Zone Requirement**

EBS volumes must be created in a specific Availability Zone, such as:

```hcl id="x7p1md"
availability_zone = "us-east-1a"
```



### **2. Incorrect Volume Type**

Ensure the volume type is exactly:

```text id="q2n8ks"
gp3
```

Using another type may fail validation.



### **3. Wrong Volume Size**

The task specifically requires:

```text id="r5v4ty"
2 GiB
```

So ensure:

```hcl id="b9m6qp"
size = 2
```

is correctly configured.



## Fun Message

*"Your cloud storage is now provisioned with Terraform — persistent, scalable, and ready for action 💾☁️🚀"*
