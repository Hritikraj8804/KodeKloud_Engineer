# Instruction

The DevOps team has been tasked with creating a secure DynamoDB table and enforcing fine-grained access control using IAM. This setup will allow secure and restricted access to the table from trusted AWS services only.

As a member of the Nautilus DevOps Team, your task is to perform the following using Terraform:

1. Create a DynamoDB Table: Create a table named `devops-table` with minimal configuration.

2. Create an IAM Role: Create an IAM role named `devops-role` that will be allowed to access the table.

3. Create an IAM Policy: Create a policy named `devops-readonly-policy` that should grant read-only access (GetItem, Scan, Query) to the specific DynamoDB table and attach it to the role.

4. Create the `main.tf` file (do not create a separate .tf file) to provision the table, role, and policy.

5. Create the `variables.tf` file with the following variables:

    - `KKE_TABLE_NAME`: name of the DynamoDB table
    - `KKE_ROLE_NAME`: name of the IAM role
    - `KKE_POLICY_NAME`: name of the IAM policy

6. Create the `outputs.tf` file with the following outputs:

    - `kke_dynamodb_table`: name of the DynamoDB table
    - `kke_iam_role_name`: name of the IAM role
    - `kke_iam_policy_name`: name of the IAM policy

7. Define the actual values for these variables in the `terraform.tfvars` file.

8. Ensure that the IAM policy allows only read access and restricts it to the specific DynamoDB table created.

Notes:

1. The Terraform working directory is `/home/bob/terraform`.

2. Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

3. Before submitting the task, ensure that terraform plan returns No changes. Your infrastructure matches the configuration.

# Solution


4. Create the `main.tf` file (do not create a separate .tf file) to provision the table, role, and policy.

5. Create the `variables.tf` file with the following variables:

    - `KKE_TABLE_NAME`: name of the DynamoDB table
    - `KKE_ROLE_NAME`: name of the IAM role
    - `KKE_POLICY_NAME`: name of the IAM policy

6. Create the `outputs.tf` file with the following outputs:

    - `kke_dynamodb_table`: name of the DynamoDB table
    - `kke_iam_role_name`: name of the IAM role
    - `kke_iam_policy_name`: name of the IAM policy

7. Define the actual values for these variables in the `terraform.tfvars` file.

8. Ensure that the IAM policy allows only read access and restricts it to the specific DynamoDB table created.

Notes:

1. The Terraform working directory is `/home/bob/terraform`.

2. Right-click under the EXPLORER section in VS Code and select Open in Integrated Terminal to launch the terminal.

3. Before submitting the task, ensure that terraform plan returns No changes. Your infrastructure matches the configuration.

## Steps

1. Let's create the `terraform.tfvars` with these values:

    ```hcl
    KKE_TABLE_NAME = "devops-table"
    KKE_ROLE_NAME = "devops-role"
    KKE_POLICY_NAME = "devops-readonly-policy"
    ```

2. Let's create the `variables.tf` for these variable:

    ```hcl
    variable "KKE_TABLE_NAME" {}
    variable "KKE_ROLE_NAME" {}
    variable "KKE_POLICY_NAME" {}
    ```

3. Now, let's create the `main.tf` file

    ```hcl
    resource "aws_dynamodb_table" "kk_dynamodb" {
      name         = var.KKE_TABLE_NAME
      hash_key     = "id"
      billing_mode = "PAY_PER_REQUEST"
    
      attribute {
        name = "id"
        type = "S"
      }
    
      tags = {
        Name = var.KKE_TABLE_NAME
      }
    }
    
    resource "aws_iam_role" "kk_role" {
      name = var.KKE_ROLE_NAME
    
      assume_role_policy = jsonencode({
        Version = "2012-10-17"
        Statement = [
          {
            Action = "sts:AssumeRole"
            Effect = "Allow"
            Sid    = ""
            Principal = {
              Service = "ec2.amazonaws.com"
            }
          },
        ]
      })
    
      tags = {
        Name = var.KKE_ROLE_NAME
      }
    }
    
    resource "aws_iam_policy" "kk_policy" {
      name        = var.KKE_POLICY_NAME
      path        = "/"
      description = "My test policy"
    
      policy = jsonencode({
        Version = "2012-10-17"
        Statement = [
          {
            Action = [
              "dynamodb:DescribeImport",
              "dynamodb:DescribeContributorInsights",
              "dynamodb:ListTagsOfResource",
              "dynamodb:GetAbacStatus",
              "dynamodb:DescribeReservedCapacityOfferings",
              "dynamodb:PartiQLSelect",
              "dynamodb:DescribeTable",
              "dynamodb:GetItem",
              "dynamodb:DescribeContinuousBackups",
              "dynamodb:DescribeExport",
              "dynamodb:GetResourcePolicy",
              "dynamodb:DescribeKinesisStreamingDestination",
              "dynamodb:DescribeLimits",
              "dynamodb:BatchGetItem",
              "dynamodb:ConditionCheckItem",
              "dynamodb:Scan",
              "dynamodb:Query",
              "dynamodb:DescribeStream",
              "dynamodb:DescribeTimeToLive",
              "dynamodb:ListStreams",
              "dynamodb:DescribeGlobalTableSettings",
              "dynamodb:GetShardIterator",
              "dynamodb:DescribeGlobalTable",
              "dynamodb:DescribeReservedCapacity",
              "dynamodb:DescribeBackup",
              "dynamodb:DescribeEndpoints",
              "dynamodb:GetRecords",
              "dynamodb:DescribeTableReplicaAutoScaling"
            ]
            Effect = "Allow"
            Resource = [
              aws_dynamodb_table.kk_dynamodb.arn,
              "${aws_dynamodb_table.kk_dynamodb.arn}/*"
            ]
          },
        ]
      })
    
      tags = {
        Name = var.KKE_POLICY_NAME
      }
    }
    
    resource "aws_iam_role_policy_attachment" "test-attach" {
      role       = aws_iam_role.kk_role.name
      policy_arn = aws_iam_policy.kk_policy.arn
    }
    ```

4. Now, let's create the `outputs.tf` file with these contents:

    ```hcl
    output "kke_dynamodb_table" {
        value = aws_dynamodb_table.kk_dynamodb.name
    }

    output "kke_iam_role_name" {
        value = aws_iam_role.kk_role.name
    }

    output "kke_iam_policy_name" {
        value = aws_iam_policy.kk_policy.name
    }
    ```

5. Let's run the terraform action commands:

    ```sh
    terraform init
    terraform plan
    terraform apply -auto-approve
    ```

    <img width="711" height="492" alt="image" src="https://github.com/user-attachments/assets/f59c6dfd-a8a3-42e0-a712-8c6d8376c46d" />

