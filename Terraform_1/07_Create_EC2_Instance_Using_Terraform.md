# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

For this task, create an EC2 instance using Terraform with the following requirements:

* The EC2 instance must use the value `xfusion-ec2` as its Name tag.
* Use the Amazon Linux AMI:

```text id="u2v8rx"
ami-0c101f26f147fa7fd
```

* Instance type must be:

```text id="n5q3md"
t2.micro
```

* Create a new RSA key named:

```text id="h7w1kp"
xfusion-kp
```

* Attach the default security group.
* Terraform working directory:

```text id="f9m4zs"
/home/bob/terraform
```

* Create only the `main.tf` file.

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="r8k2yv"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="m4v7qd"
vi main.tf
```

OR

```bash id="c9x5tp"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="w6n1fk"
resource "tls_private_key" "xfusion_key" {
  algorithm = "RSA"
  rsa_bits  = 2048
}

resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = tls_private_key.xfusion_key.public_key_openssh
}

data "aws_security_group" "default" {
  name = "default"
}

resource "aws_instance" "xfusion_ec2" {
  ami                    = "ami-0c101f26f147fa7fd"
  instance_type          = "t2.micro"
  key_name               = aws_key_pair.xfusion_kp.key_name
  vpc_security_group_ids = [data.aws_security_group.default.id]

  tags = {
    Name = "xfusion-ec2"
  }
}
```

<img width="915" height="606" alt="main" src="https://github.com/user-attachments/assets/60ef7a9d-db1b-4895-91ad-09d59a7a05e0" />


### **4. Initialize Terraform**

Initialize Terraform and download the required providers:

```bash id="t3p8zn"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash id="j6m2vc"
terraform validate
```

Expected output:

```bash id="d8q5ls"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the EC2 instance using:

```bash id="k1w7rx"
terraform apply
```

Type:

```bash id="y4n9tb"
yes
```

when prompted.



### **7. Verify the EC2 Instance**

Verify the instance using AWS CLI:

```bash id="v2m6pd"
aws ec2 describe-instances
```

Or verify from the AWS Console:

* **EC2 Dashboard**
* **Instances**
* Search for:

```text id="x5r8ka"
xfusion-ec2
```

You should also see the key pair:

```text id="q7t1mw"
xfusion-kp
```

<img width="762" height="287" alt="res" src="https://github.com/user-attachments/assets/2af67912-a206-4fd3-ab08-ad29d1489023" />


## Terraform Resources Used

| Resource                         | Purpose                            |
| -------------------------------- | ---------------------------------- |
| `tls_private_key`                | Generates RSA key pair locally     |
| `aws_key_pair`                   | Uploads public key to AWS          |
| `aws_instance`                   | Launches EC2 instance              |
| `aws_security_group` data source | Fetches the default security group |



## Challenges

### **1. Wrong AMI ID**

Always use the exact AMI:

```text id="e9v3qs"
ami-0c101f26f147fa7fd
```

Using another AMI may fail validation.



### **2. Missing Default Security Group**

The task specifically requires the default security group. Ensure:

```hcl id="n4x7kc"
data "aws_security_group" "default"
```

is used correctly.



### **3. Forgetting Key Pair Creation**

The EC2 instance requires the key pair:

```text id="p6m1wd"
xfusion-kp
```

Without it, instance creation may fail.



## Fun Message

*"Terraform just launched your first cloud machine — infrastructure as code in action 🚀☁️"*
