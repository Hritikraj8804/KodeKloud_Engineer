# Instruction

The Nautilus DevOps team needs to create an AWS Kinesis Data Stream for real-time data processing. This stream will be used to ingest and process large volumes of streaming data, enabling analytics and real-time decision-making across various applications.

For this task:

* Create an AWS Kinesis Data Stream named `nautilus-stream`
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file
* Before submitting, ensure:

```bash
terraform plan
```

returns:

```text
No changes. Your infrastructure matches the configuration.
```

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

resource "aws_kinesis_stream" "nautilus_stream" {
  name             = "nautilus-stream"
  shard_count      = 1
  retention_period = 24

  tags = {
    Name = "nautilus-stream"
  }
}
```

<img width="922" height="362" alt="main" src="https://github.com/user-attachments/assets/fa70c7c6-426e-4ade-b7b0-0a817bf84640" />


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

Create the Kinesis Data Stream:

```bash
terraform apply -auto-approve
```

Terraform will:

* Create the Kinesis stream
* Configure one shard
* Set the default retention period of 24 hours



### **7. Confirm Terraform State**

Before submitting the task, run:

```bash
terraform plan
```

Expected output:

```text
No changes. Your infrastructure matches the configuration.
```

This confirms that the deployed infrastructure matches the Terraform configuration exactly.

<img width="775" height="211" alt="approvw" src="https://github.com/user-attachments/assets/fd9d53d0-9540-4490-9ee9-36194c64cb6e" />


### **8. Verify the Kinesis Stream**

Verify using AWS CLI:

```bash
aws kinesis describe-stream-summary --stream-name nautilus-stream
```

Or verify from the AWS Console:

* **Amazon Kinesis**
* **Data Streams**
* Search for:

```text
nautilus-stream
```

The stream status should be:

```text
ACTIVE
```



## Terraform Resources Used

| Resource             | Purpose                               |
| -------------------- | ------------------------------------- |
| `aws_kinesis_stream` | Creates an Amazon Kinesis Data Stream |



## Challenges

### **1. Minimum Shard Requirement**

A Kinesis stream requires at least one shard:

```hcl
shard_count = 1
```



### **2. Stream Creation Takes Time**

After applying Terraform, AWS may take a short time before the stream status becomes:

```text
ACTIVE
```



### **3. Verify Infrastructure State**

Always run:

```bash
terraform plan
```

before submitting.

The task explicitly requires:

```text
No changes. Your infrastructure matches the configuration.
```



## Fun Message

*"Your real-time data pipeline is now flowing through AWS Kinesis—streaming events at cloud speed ⚡🌊☁️"*
