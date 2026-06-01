# Instruction

The Nautilus DevOps team needs to create an AWS Systems Manager (SSM) Parameter Store entry to securely store configuration data.

For this task:

* Create an SSM parameter named `datacenter-ssm-parameter`
* Parameter type must be `String`
* Parameter value must be `datacenter-value`
* Create the parameter in the `us-east-1` region
* Use Terraform
* Ensure the parameter can be retrieved after creation
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

resource "aws_ssm_parameter" "datacenter_parameter" {
  name  = "datacenter-ssm-parameter"
  type  = "String"
  value = "datacenter-value"
}
```

<img width="906" height="260" alt="main" src="https://github.com/user-attachments/assets/d4497370-d6e7-4831-82d8-3a7749f73b44" />


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

Create the SSM parameter using:

```bash
terraform apply -auto-approve
```

Terraform will:

* Connect to AWS
* Create the SSM parameter
* Store the parameter in AWS Systems Manager Parameter Store

<img width="808" height="165" alt="list" src="https://github.com/user-attachments/assets/be9ddc31-666e-4c90-9e6f-f6fc54eea6f2" />


### **7. Verify the SSM Parameter**

Verify the parameter using AWS CLI:

```bash
aws ssm get-parameter \
  --name datacenter-ssm-parameter \
  --region us-east-1
```

Expected output should contain:

```text
Name: datacenter-ssm-parameter
Type: String
Value: datacenter-value
```

Or verify from the AWS Console:

* **AWS Systems Manager**
* **Parameter Store**
* Search for:

```text
datacenter-ssm-parameter
```

<img width="756" height="295" alt="verify" src="https://github.com/user-attachments/assets/d64c6873-0f86-4a4f-9fb0-60d0ddbfdf96" />


## Terraform Resources Used

| Resource            | Purpose                             |
| ------------------- | ----------------------------------- |
| `aws_ssm_parameter` | Creates a Parameter Store parameter |



## Challenges

### **1. Correct Parameter Type**

The task specifically requires:

```hcl
type = "String"
```

Using `SecureString` or `StringList` may fail validation.



### **2. Exact Parameter Name**

Ensure the parameter name exactly matches:

```text
datacenter-ssm-parameter
```

Terraform validation tasks are often case-sensitive.



### **3. Region Verification**

The parameter must be created in:

```hcl
region = "us-east-1"
```

Otherwise the task validation may fail.



## Fun Message

*"Your configuration is now safely stored in Parameter Store—centralized, manageable, and ready for your applications 🔧☁️🚀"*
