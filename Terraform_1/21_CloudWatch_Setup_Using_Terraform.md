# Instruction

The Nautilus DevOps team needs to set up CloudWatch logging for their application. They need to create a CloudWatch Log Group and Log Stream to store and organize application logs.

For this task:

* Create a CloudWatch Log Group named `datacenter-log-group`
* Create a CloudWatch Log Stream named `datacenter-log-stream`
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

resource "aws_cloudwatch_log_group" "datacenter_log_group" {
  name = "datacenter-log-group"
}

resource "aws_cloudwatch_log_stream" "datacenter_log_stream" {
  name           = "datacenter-log-stream"
  log_group_name = aws_cloudwatch_log_group.datacenter_log_group.name
}
```

<img width="922" height="312" alt="main" src="https://github.com/user-attachments/assets/b8878c38-5ed0-474d-ad5a-3d278b471c15" />


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

Create the CloudWatch Log Group and Log Stream:

```bash
terraform apply -auto-approve
```

Terraform will:

* Create the CloudWatch Log Group
* Create the CloudWatch Log Stream within that log group
* Store the resource state locally

<img width="795" height="117" alt="apply" src="https://github.com/user-attachments/assets/0368b854-b84f-481d-aa09-a1ce107b1182" />


### **7. Verify the Resources**

Verify the Log Group:

```bash
aws logs describe-log-groups \
  --log-group-name-prefix datacenter-log-group
```

Verify the Log Stream:

```bash
aws logs describe-log-streams \
  --log-group-name datacenter-log-group
```

Expected resources:

```text
Log Group  : datacenter-log-group
Log Stream : datacenter-log-stream
```

Or verify from the AWS Console:

* **CloudWatch**
* **Logs**
* **Log Groups**
* Open:

```text
datacenter-log-group
```

* Verify the stream:

```text
datacenter-log-stream
```

<img width="637" height="72" alt="list" src="https://github.com/user-attachments/assets/7417d9b4-f295-4134-96aa-5418cc8880af" />


## Terraform Resources Used

| Resource                    | Purpose                                   |
| --------------------------- | ----------------------------------------- |
| `aws_cloudwatch_log_group`  | Creates a CloudWatch Log Group            |
| `aws_cloudwatch_log_stream` | Creates a Log Stream within the Log Group |



## Challenges

### **1. Log Stream Depends on Log Group**

The log stream cannot be created until the log group exists:

```hcl
log_group_name = aws_cloudwatch_log_group.datacenter_log_group.name
```

Terraform automatically handles this dependency.



### **2. Exact Resource Names**

Ensure the names exactly match:

```text
datacenter-log-group
```

and

```text
datacenter-log-stream
```

Task validation is typically case-sensitive.



### **3. Verify in CloudWatch**

After applying, always verify that both resources exist before submitting the task.



## Fun Message

*"Your application now has a home for its logs—CloudWatch is ready to capture every event, error, and success story 📜☁️🚀"*
