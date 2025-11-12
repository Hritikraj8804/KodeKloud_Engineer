# Instruction


The Nautilus DevOps team is expanding their AWS infrastructure and requires the setup of a private Virtual Private Cloud (VPC) along with a subnet. This VPC and subnet configuration will ensure that resources deployed within them remain isolated from external networks and can only communicate within the VPC. Additionally, the team needs to provision an EC2 instance under the newly created private VPC. This instance should be accessible only from within the VPC, allowing for secure communication and resource management within the AWS environment.

1. Create a VPC named `datacenter-priv-vpc` with the CIDR block `10.0.0.0/16`.

2. Create a subnet named `datacenter-priv-subnet` inside the VPC with the CIDR block `10.0.1.0/24` and auto-assign IP option must not be enabled.

3. Create an EC2 instance named `datacenter-priv-ec2` inside the subnet and instance type must be `t2.micro`.

4. Ensure the security group of the EC2 instance allows access only from within the `VPC's CIDR block`.

5. Create the `main.tf` file (do not create a separate .tf file) to provision the VPC, subnet and EC2 instance.

6. Use `variables.tf` file with the following variable names:

    - `KKE_VPC_CIDR` for the VPC CIDR block.
    - `KKE_SUBNET_CIDR` for the subnet CIDR block.

7. Use the `outputs.tf` file with the following variable names:

    - `KKE_vpc_name` for the name of the VPC.
    - `KKE_subnet_name` for the name of the subnet.
    - `KKE_ec2_private` for the name of the EC2 instance.

> Notes:

- The Terraform working directory is /home/bob/terraform.

- Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

- Before submitting the task, ensure that terraform plan returns No changes. Your infrastructure matches the configuration.


# Solution


1. Create the `variables.tf` with the following contents:

    ```hcl
    variable "KKE_VPC_CIDR" {
        default = "10.0.0.0/16"
    }

    variable "KKE_SUBNET_CIDR" {
        default = "10.0.1.0/24"
    }

    variable "prefix" {
        default = "datacenter-priv"
    }
    ```

    > Here we have added `prefix` to set name for each resource

2. Create the `main.tf` with the following contents:

    ```hcl
    resource "aws_vpc" "vpc" {
        cidr_block = var.KKE_VPC_CIDR

        tags = {
            Name = "${var.prefix}-vpc"
        }
    }

    resource "aws_subnet" "subnet" {
        cidr_block = var.KKE_SUBNET_CIDR
        vpc_id = aws_vpc.vpc.id
        map_public_ip_on_launch = false

        tags = {
            Name = "${var.prefix}-subnet"
        }
    }

    resource "aws_security_group" "sg" {
        vpc_id = aws_vpc.vpc.id
        name = "${var.prefix}-sg"
        description = "Security group for EC2 instance"

        tags = {
            Name = "${var.prefix}-sg"
        }
    }

    resource "aws_vpc_security_group_ingress_rule" "allow_http" {
        security_group_id = aws_security_group.sg.id
        from_port = 80
        to_port = 80
        ip_protocol = "tcp"
        cidr_ipv4 = var.KKE_VPC_CIDR
    }

    resource "aws_vpc_security_group_ingress_rule" "allow_ssh" {
        security_group_id = aws_security_group.sg.id
        from_port = 22
        to_port = 22
        ip_protocol = "tcp"
        cidr_ipv4 = var.KKE_VPC_CIDR
    }

    resource "aws_vpc_security_group_egress_rule" "allow_all_outbound" {
        security_group_id = aws_security_group.sg.id
        from_port = 0
        to_port = 0
        ip_protocol = "-1"
        cidr_ipv4 = "0.0.0.0/0"
    }


    data "aws_ami" "amazon_linux" {
        most_recent = true
        owners = ["amazon"]

        filter {
            name = "name"
            values = ["amzn2-ami-hvm-*-x86_64-ebs"]
        }
    }

    resource "aws_instance" "ec2" {
        subnet_id = aws_subnet.subnet.id
        instance_type = "t2.micro"
        vpc_security_group_ids = [aws_security_group.sg.id]
        ami = data.aws_ami.amazon_linux.id

        tags = {
            Name = "${var.prefix}-ec2"
        }
    }
    ```

    - Here we have added: vpc, subnet, security group, ec2 instance
    - Data source to find AMI ID Amazon Linux Image
    - Ingress rule to allow ports: 80 and 22 access only within VPC CIDR Range
    - Egress rule to go outbound request anywhere
    - in subnet: we have disabled Public IP attachment
    - Using prefix, we have set each resource name within tags
    - Here is the full [main.tf](../files/terraform_ec2_instance_launch_private_subnet_98.tf)

3. Let's create the `outputs.tf` file with the following contents:

    ```hcl
    output "KKE_vpc_name" {
        value = aws_vpc.vpc.tags["Name"]
    }

    output "KKE_subnet_name" {
        value = aws_subnet.subnet.tags["Name"]
    }

    output "KKE_ec2_private" {
        value = aws_instance.ec2.tags["Name"]
    }
    ```

    - We have used the tags to find the resource name

4. Now lets run the terraform commands:

    ```sh
    terraform init
    terraform plan
    terraform apply -auto-approve
    ```

    <img width="756" height="646" alt="Screenshot 2025-11-12 082919" src="https://github.com/user-attachments/assets/f98f1442-ec3b-40fb-a6a9-1f63f69b206b" />

    again:
    `terraform plan`
    <img width="731" height="367" alt="Screenshot 2025-11-12 083844" src="https://github.com/user-attachments/assets/458b8ea9-f521-44da-b707-6df89677e30b" />

    `terraform apply -auto-approve`
    <img width="749" height="397" alt="Screenshot 2025-11-12 083909" src="https://github.com/user-attachments/assets/afeea8ad-5041-4af0-baa9-f8c91217d3be" />

    > It will display resource names at the end:

    ```output
    KKE_ec2_private = "datacenter-priv-ec2"
    KKE_subnet_name = "datacenter-priv-subnet"
    KKE_vpc_name = "datacenter-priv-vpc"
    ```
