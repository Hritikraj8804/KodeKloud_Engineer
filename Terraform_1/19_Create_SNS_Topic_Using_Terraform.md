# Instruction

The Nautilus DevOps team needs to set up an SNS topic for sending notifications. Amazon SNS (Simple Notification Service) enables applications and services to send messages to subscribers through various communication channels.

For this task:

* Create an SNS topic named `xfusion-notifications`
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

resource "aws_sns_topic" "xfusion_notifications" {
  name = "xfusion-notifications"
}
```

<img width="853" height="177" alt="main" src="https://github.com/user-attachments/assets/8dd02279-0535-4cec-83e3-70264dfae9c6" />


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

Create the SNS topic using:

```bash
terraform apply -auto-approve
```

Terraform will:

* Connect to AWS
* Create the SNS topic
* Store the topic ARN in the Terraform state



### **7. Verify the SNS Topic**

Verify the SNS topic using AWS CLI:

```bash
aws sns list-topics
```

Look for a topic ARN containing:

```text
xfusion-notifications
```

Or verify from the AWS Console:

* **Amazon SNS**
* **Topics**
* Search for:

```text
xfusion-notifications
```

<img width="765" height="251" alt="list" src="https://github.com/user-attachments/assets/cf4985bb-6765-4745-908c-e3c9a7579011" />


## Terraform Resources Used

| Resource        | Purpose                     |
| --------------- | --------------------------- |
| `aws_sns_topic` | Creates an Amazon SNS topic |



## Challenges

### **1. Correct Topic Name**

Ensure the SNS topic name is exactly:

```text
xfusion-notifications
```

Terraform validation tasks are often case-sensitive.



### **2. AWS Credentials**

Terraform requires valid AWS credentials before running:

```bash
terraform apply
```

Otherwise the SNS topic creation will fail.



### **3. Verify the Topic ARN**

After creation, AWS automatically generates an ARN similar to:

```text
arn:aws:sns:us-east-1:123456789012:xfusion-notifications
```

This ARN is used when creating subscriptions and publishing messages.



## Fun Message

*"Your notification hub is ready! SNS can now broadcast messages to applications, emails, queues, and more 📢☁️🚀"*
