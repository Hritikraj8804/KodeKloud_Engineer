# Create EC2 Instance Using Terraform

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units.

For this task, create an EC2 instance using Terraform with the following requirements:

1. The name of the instance must be `xfusion-ec2`.

2. Use the Amazon Linux `ami-0c101f26f147fa7fd` to launch this instance.

3. The Instance type must be `t2.micro`.

4. Create a new RSA key named `xfusion-kp`.

5. Attach the default (available by default) security group.

The Terraform working directory is /home/bob/terraform. Create the main.tf file (do not create a different .tf file) to provision the instance.

> Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

# Solution

### 📜 `main.tf`

```terraform
resource "tls_private_key" "xfusion_key_gen" {
  algorithm = "RSA"
  rsa_bits  = 4096
}

resource "aws_key_pair" "xfusion_kp" {
  key_name   = "xfusion-kp"
  public_key = tls_private_key.xfusion_key_gen.public_key_openssh
}

resource "aws_instance" "xfusion_ec2" {
  ami           = "ami-0c101f26f147fa7fd"

  instance_type = "t2.micro"

  key_name = aws_key_pair.xfusion_kp.key_name 
  
  tags = {
    Name = "xfusion-ec2"
  }
}
```

-----

### ⏭️ Next Steps

After saving this file in `/home/bob/terraform`, run the following commands in your terminal:

1.  **Initialize:** `terraform init`
2.  **Plan:** `terraform plan`
3.  **Apply:** `terraform apply -auto-approve`
