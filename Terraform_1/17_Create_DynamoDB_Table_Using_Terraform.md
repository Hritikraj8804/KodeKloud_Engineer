# Instruction

The Nautilus DevOps team needs to set up a DynamoDB table for storing user data. They have provided the following requirements:

* Create a DynamoDB table named `nautilus-users`
* The primary key should be `nautilus_id`
* The primary key type should be `String`
* Use `PAY_PER_REQUEST` billing mode
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash
vi main.tf
```

OR

```bash
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_dynamodb_table" "nautilus_users" {
  name         = "nautilus-users"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "nautilus_id"

  attribute {
    name = "nautilus_id"
    type = "S"
  }

  tags = {
    Name = "nautilus-users"
  }
}
```

<img width="856" height="351" alt="main" src="https://github.com/user-attachments/assets/8cf70511-ec1a-4473-8ae5-2f859682b3a5" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the DynamoDB table using:

```bash
terraform apply -auto-approve
```

Terraform will:

* Create the DynamoDB table
* Configure the primary key
* Enable on-demand billing mode (`PAY_PER_REQUEST`)



### **7. Verify the DynamoDB Table**

Verify the table using AWS CLI:

```bash
aws dynamodb describe-table --table-name nautilus-users
```

Look for:

```text
TableName: nautilus-users
BillingMode: PAY_PER_REQUEST
HashKey: nautilus_id
```

Or verify from the AWS Console:

* **DynamoDB Dashboard**
* **Tables**
* Search for:

```text
nautilus-users
```



## Terraform Resources Used

| Resource             | Purpose                  |
| -------------------- | ------------------------ |
| `aws_dynamodb_table` | Creates a DynamoDB table |

<img width="726" height="232" alt="list" src="https://github.com/user-attachments/assets/a8050402-501b-4622-b7f9-49feda4d3cd6" />


## Challenges

### **1. Correct Primary Key Type**

The task requires:

```text
nautilus_id -> String
```

So the attribute type must be:

```hcl
type = "S"
```

Where:

* `S` = String
* `N` = Number
* `B` = Binary



### **2. Correct Billing Mode**

Ensure the billing mode is:

```hcl
billing_mode = "PAY_PER_REQUEST"
```

Using `PROVISIONED` may cause the validation to fail.



### **3. Exact Table Name**

The table name must exactly match:

```text
nautilus-users
```

Terraform validation tasks are often case-sensitive.



## Fun Message

*"Your DynamoDB table is now serverless, scalable, and ready to store millions of records without managing a single server 🚀📊☁️"*
