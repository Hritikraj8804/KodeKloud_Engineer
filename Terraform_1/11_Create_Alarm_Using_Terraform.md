# Instruction

The Nautilus DevOps team is setting up monitoring in their AWS account. As part of this, they need to create a CloudWatch alarm.

For this task:

* Create a CloudWatch alarm named `devops-alarm`
* Monitor CPU utilization of an EC2 instance
* Trigger the alarm when CPU utilization exceeds `80%`
* Set the evaluation period to `5 minutes`
* Use a single evaluation period
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="v7m2qx"
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash id="p4k8zd"
vi main.tf
```

OR

```bash id="r9x3wc"
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl id="t5n1fj"
provider "aws" {
  region = "us-east-1"
}

# Fetch existing EC2 instance
data "aws_instances" "ec2_instances" {}

# Create CloudWatch Alarm
resource "aws_cloudwatch_metric_alarm" "devops_alarm" {
  alarm_name          = "devops-alarm"
  comparison_operator = "GreaterThanThreshold"
  evaluation_periods  = 1
  metric_name         = "CPUUtilization"
  namespace           = "AWS/EC2"
  period              = 300
  statistic           = "Average"
  threshold           = 80
  alarm_description   = "Alarm when CPU exceeds 80%"

  dimensions = {
    InstanceId = data.aws_instances.ec2_instances.ids[0]
  }
}
```

<img width="950" height="360" alt="main" src="https://github.com/user-attachments/assets/2bb8e906-b367-4bca-9dce-1c38a5230ecb" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash id="j6v9tr"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash id="m2x7qp"
terraform validate
```

Expected output:

```bash id="f8k4wd"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the CloudWatch alarm using:

```bash id="n5r1zc"
terraform apply -auto-approve
```

Terraform will:

* Fetch the existing EC2 instance
* Create the CloudWatch CPU alarm
* Configure the monitoring threshold

<img width="757" height="587" alt="apply" src="https://github.com/user-attachments/assets/b8a8da8b-e228-47b7-bf01-d58699298664" />


### **7. Verify the CloudWatch Alarm**

Verify the alarm using AWS CLI:

```bash id="q3m8vk"
aws cloudwatch describe-alarms --alarm-names devops-alarm
```

Or verify from the AWS Console:

* **CloudWatch Dashboard**
* **Alarms**
* Search for:

```text id="u9p4xs"
devops-alarm
```

You should see:

* Metric: `CPUUtilization`
* Threshold: `80`
* Period: `300 seconds`
* Evaluation Periods: `1`

<img width="640" height="70" alt="list" src="https://github.com/user-attachments/assets/7a61f2e4-f3a3-4b3f-99bf-5b2d749c29c0" />


## Terraform Resources Used

| Resource                      | Purpose                           |
| ----------------------------- | --------------------------------- |
| `aws_cloudwatch_metric_alarm` | Creates the CloudWatch alarm      |
| `aws_instances` data source   | Fetches existing EC2 instance IDs |



## Challenges

### **1. Incorrect Evaluation Period**

The task specifically requires:

* `5 minutes`
* `1 evaluation period`

So ensure:

```hcl id="w1n7yd"
period             = 300
evaluation_periods = 1
```



### **2. Wrong Metric Namespace**

For EC2 CPU monitoring, always use:

```hcl id="k4r9pm"
namespace = "AWS/EC2"
```



### **3. Missing Instance ID**

CloudWatch alarms require a valid EC2 instance ID:

```hcl id="x6m2tf"
InstanceId = data.aws_instances.ec2_instances.ids[0]
```



## Fun Message

*"Your infrastructure can now alert you before the servers start melting under CPU pressure 🔥📈☁️"*
