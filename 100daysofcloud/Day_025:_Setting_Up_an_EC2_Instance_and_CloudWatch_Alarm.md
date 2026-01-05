# Instruction

The Nautilus DevOps team has been tasked with setting up an EC2 instance for their application. To ensure the application performs optimally, they also need to create a CloudWatch alarm to monitor the instance's CPU utilization. The alarm should trigger if the CPU utilization exceeds 90% for one consecutive 5-minute period. To send notifications, use the SNS topic named devops-sns-topic which is already created.
Launch EC2 Instance: Create an EC2 instance named `devops-ec2` using any appropriate Ubuntu AMI.

Create CloudWatch Alarm: Create a CloudWatch alarm named `devops-alarm` with the following specifications:
Statistic: `Average`
Metric: CPU Utilization
Threshold: `>= 90%` for 1 consecutive 5-minute period.
Alarm Actions: Send a notification to `devops-sns-topic`.


# Solution


### Step 1: Launch the EC2 Instance

1. **Open EC2 Dashboard**: Log in to the AWS Management Console and navigate to EC2.
2. **Launch Instance**: Click **Launch instance**.
3. **Name and Tags**:
* **Name**: `devops-ec2`
4. **Application and OS Image (AMI)**:
* Select **Ubuntu** (e.g., Ubuntu Server 24.04 LTS).
5. **Instance Type**: Select an appropriate type (e.g., `t2.micro` or `t3.micro`).
6. **Finalize**: Select your key pair and network settings, then click **Launch instance**.
7. **Copy Instance ID**: Once launched, go to the instances list and **copy the Instance ID** (e.g., `i-0abcd1234efgh5678`). You will need this for the alarm.

![1](https://github.com/user-attachments/assets/56e23d24-78e5-4c90-91c2-eed54d01d294)

### Step 2: Create the CloudWatch Alarm

1. **Navigate to CloudWatch**: Search for **CloudWatch** in the AWS console.
2. **Create Alarm**: In the left menu, go to **Alarms** > **All alarms** and click **Create alarm**.

  ![3](https://github.com/user-attachments/assets/f6c57195-c400-4a02-9b08-d4c9927bbe19)

3. **Select Metric**:
* Click **Select metric**.
* Choose **EC2** > **Per-Instance Metrics**.
  
  ![4](https://github.com/user-attachments/assets/93363861-dddf-489b-9c73-c773dacf7d34)

* **Crucial Fix**: Paste your **Instance ID** into the search bar. Look for the metric named **CPUUtilization** for that specific ID.

  ![5](https://github.com/user-attachments/assets/f120f49a-e90c-493d-ada1-0304b41cc40a)

* Click **Select metric**.
4. **Conditions**:
* **Statistic**: Average
* **Period**: 5 minutes

  ![6](https://github.com/user-attachments/assets/8fd6defd-e0ad-4076-9c63-910d3d93498a)

* **Threshold type**: Static
* **Condition**: Whenever CPUUtilization is... **Greater/Equal (>=)** than **90**.
* **Datapoints to alarm**: 1 out of 1.

  ![7](https://github.com/user-attachments/assets/79421026-a4d9-482f-ba7e-a84ab4055839)


### Step 3: Configure Notifications (SNS)

1. **Configure Actions**: Under the **Notification** section, select the **In alarm** state trigger.
2. **Select SNS Topic**: Choose **Select an existing SNS topic**.
3. **Topic Name**: Search for and select `devops-sns-topic`.
4. **Name and Description**:
* **Alarm name**: `devops-alarm`
5. **Finalize**: Review the configuration and click **Create alarm**.

  ![8](https://github.com/user-attachments/assets/e2f793fd-c846-41d6-8e00-81a8b7084976)

  ![9](https://github.com/user-attachments/assets/8fc083bd-4311-4ef2-9a33-c213d36813f2)

  ![10](https://github.com/user-attachments/assets/30e0c762-dc3e-4c1a-a20c-805619342e48)

---

### Key Technical Considerations

* **Metric Visibility**: It may take a few minutes after launching an instance for its metrics to appear in CloudWatch. If you don't see the Instance ID immediately, wait 2–3 minutes.
* **Evaluation Period**: By setting the period to 5 minutes and 1 consecutive period, the alarm will trigger as soon as the *average* CPU of a 5-minute window hits 90%.
* **SNS Subscription**: Ensure that your email or endpoint is actually **confirmed** in the SNS topic settings, otherwise, you won't receive the alert even if the alarm triggers!

---

### Fun Fact

> Think of a CloudWatch Alarm like a smoke detector in a kitchen. The "Metric" is the smoke level, and the "Threshold" is the point where the alarm starts beeping. The SNS topic is like the phone call the security company makes to your cell phone to tell you there's a fire!

[Dev.to](https://dev.to/hritikraj8804/aws-125-guardian-of-the-cloud-setting-up-cloudwatch-alarms-43lj)
