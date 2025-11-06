## instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.  
To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations.  
By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.  

Create a VPC named `datacenter-vpc` in region `us-east-1` with any IPv4 CIDR block through terraform.  

The Terraform working directory is `/home/bob/terraform`. Create the main.tf file (do not create a different .tf file) to accomplish this task.  


Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.  


## solution

Knowing that the `provider.tf` file is already set up to point to a local development environment (LocalStack, perhaps, given the `http://aws:4566` endpoints) simplifies things—you only need the resource definition in `main.tf`.

goal: create a VPC named `datacenter-vpc` in region `us-east-1` (which is already configured by your provider file) with any IPv4 CIDR block.

You need to create the file `/home/bob/terraform/main.tf` with just the VPC resource:

### 📜 `main.tf`

```terraform
# Resource for the VPC named 'datacenter-vpc'
resource "aws_vpc" "datacenter" {
  # Using a standard private CIDR block; you can choose any valid block
  cidr_block = "10.10.0.0/16" 

  tags = {
    Name = "datacenter-vpc"
  }
}
```

### Next Steps:

1.  **Initialize (if not done already):** You'll need to run `terraform init` to set up the AWS provider based on your provided configuration.
2.  **Plan:** Run `terraform plan` to see what changes will be applied.
3.  **Apply:** Run `terraform apply -auto-approve` to create the VPC in your targeted local environment.

<img width="998" height="772" alt="image" src="https://github.com/user-attachments/assets/a37ef86c-7394-4f5b-99be-7348eca3e072" />
