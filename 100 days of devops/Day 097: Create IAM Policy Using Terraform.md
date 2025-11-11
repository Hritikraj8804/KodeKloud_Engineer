# Instruction

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements.

Create an IAM policy named `iampolicy_jim` in `us-east-1` region using Terraform. It must allow read-only access to the `EC2 console`, i.e., this policy must allow users to view `all instances`, `AMIs`, and `snapshots` in the Amazon EC2 console.

The Terraform working directory is `/home/bob/terraform`. Create the `main.tf` file (do not create a different .tf file) to accomplish this task.

> Note: Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

# Solution

## Steps

1. Create a `main.tf` file with the following contents:

```hcl
    resource "aws_iam_policy" "policy" {
        name        = var.policy_name
        path        = "/"
        description = "My test policy"

        policy = jsonencode({
            Version = "2012-10-17"
            Statement = [
            {
                Action = [
                "ec2:Describe*",
                ]
                Effect   = "Allow"
                Resource = "*"
            },
            ]
        })

        tags = {
            Name = var.policy_name
        }

    }
    
  ```

2. Let's define the variable by creating a `variables.tf` file:

    ```hcl
    variable "policy_name" {
        default = "iampolicy_jim" 
    }
    ```

    > Make sure you have changed the value here according to your task description



### Key Logic:

  * **`data "aws_iam_policy_document"`**: This resource is used to construct the policy statement in HCL (HashiCorp Configuration Language), which Terraform then renders as the required JSON.
  * **`ec2:Describe*`**: This action covers all API calls that start with `Describe` (e.g., `DescribeInstances`, `DescribeImages`, `DescribeSnapshots`), effectively granting read-only access across the entire EC2 console.
  * **`resources = ["*"]`**: Applies the read-only permission to all resources within the EC2 service.

3. Run the terraform commands:

    ```sh
    terraform init
    terraform plan
    terraform apply -auto-approve
    ```

<img width="662" height="541" alt="image" src="https://github.com/user-attachments/assets/9c872e09-fe8d-49ce-8129-3bb36465715a" />
