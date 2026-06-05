# Instruction

The Nautilus DevOps team needs to store sensitive data securely using AWS Secrets Manager. AWS Secrets Manager helps manage, retrieve, and rotate secrets such as database credentials, API keys, and application passwords.

For this task:

* Create a secret named `devops-secret`
* Store the following key-value pairs:

  * `username: admin`
  * `password: Namin123`
* Use Terraform to create the secret in AWS Secrets Manager
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

resource "aws_secretsmanager_secret" "devops_secret" {
  name = "devops-secret"
}

resource "aws_secretsmanager_secret_version" "devops_secret_value" {
  secret_id = aws_secretsmanager_secret.devops_secret.id

  secret_string = jsonencode({
    username = "admin"
    password = "Namin123"
  })
}
```

<img width="955" height="342" alt="config" src="https://github.com/user-attachments/assets/266b442b-87be-4d3f-a05a-bb18fa82f2bc" />


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

Create the secret using:

```bash
terraform apply -auto-approve
```

Terraform will:

* Create the Secrets Manager secret
* Store the username and password as a JSON secret value
* Create the initial secret version

<img width="640" height="207" alt="list" src="https://github.com/user-attachments/assets/7c794ca2-c085-4b04-863d-e5b572f6f8e0" />


### **7. Verify the Secret**

Verify the secret metadata:

```bash
aws secretsmanager describe-secret \
  --secret-id devops-secret
```

Retrieve the secret value:

```bash
aws secretsmanager get-secret-value \
  --secret-id devops-secret
```

Expected secret content:

```json
{
  "username": "admin",
  "password": "Namin123"
}
```

Or verify from the AWS Console:

* **AWS Secrets Manager**
* **Secrets**
* Open:

```text
devops-secret
```

* View the stored secret value



## Terraform Resources Used

| Resource                            | Purpose                            |
| ----------------------------------- | ---------------------------------- |
| `aws_secretsmanager_secret`         | Creates the Secrets Manager secret |
| `aws_secretsmanager_secret_version` | Stores the secret value            |



## Challenges

### **1. Secret Value Must Be JSON**

AWS Secrets Manager commonly stores structured data as JSON:

```hcl
secret_string = jsonencode({
  username = "admin"
  password = "Namin123"
})
```



### **2. Secret Requires a Version**

Creating only the secret container is not enough. A secret value must be stored using:

```hcl
resource "aws_secretsmanager_secret_version"
```



### **3. Verify the Stored Values**

Ensure the secret contains exactly:

```text
username = admin
password = Namin123
```

Otherwise the task validation may fail.



## Fun Message

*"Your credentials are now locked away in AWS Secrets Manager—secure, encrypted, and ready for your applications 🔐☁️🚀"*
