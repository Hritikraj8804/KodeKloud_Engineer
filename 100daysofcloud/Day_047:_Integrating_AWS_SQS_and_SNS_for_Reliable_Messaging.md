# Instruction

The Nautilus DevOps team needs to implement priority queuing using Amazon SQS and SNS. The goal is to create a system where messages with different priorities are handled accordingly. You are required to use AWS CloudFormation to deploy the necessary resources in your AWS account. The CloudFormation template should be created on the AWS client host at `/root/nautilus-priority-stack.yml`, the stack name must be `nautilus-priority-stack` and it should create the following resources:

Two SQS queues named `nautilus-High-Priority-Queue` and `nautilus-Low-Priority-Queue`.

An SNS topic named `nautilus-Priority-Queues-Topic`.

A Lambda function named `nautilus-priorities-queue-function` that will consume messages from the SQS queues. The Lambda function code is provided in `/root/index.py` on the AWS client host.

An IAM role named `lambda_execution_role` that provides the necessary permissions for the Lambda function to interact with SQS and SNS.

Once the stack is deployed, to test the same you can publish messages to the SNS topic, invoke the Lambda function and observe the order in which they are processed by the Lambda function. The high-priority message must be processed first.

```bash
topicarn=$(aws sns list-topics --query "Topics[?contains(TopicArn, 'nautilus-Priority-Queues-Topic')].TopicArn" --output text)


aws sns publish --topic-arn $topicarn --message 'High Priority message 1' --message-attributes '{"priority" : { "DataType":"String", "StringValue":"high"}}'

aws sns publish --topic-arn $topicarn --message 'High Priority message 2' --message-attributes '{"priority" : { "DataType":"String", "StringValue":"high"}}'

aws sns publish --topic-arn $topicarn --message 'Low Priority message 1' --message-attributes '{"priority" : { "DataType":"String", "StringValue":"low"}}'

aws sns publish --topic-arn $topicarn --message 'Low Priority message 2' --message-attributes '{"priority" : { "DataType":"String", "StringValue":"low"}}'

```


# Solution

---

### **Step 1: Create the IAM Role**

1. Navigate to **IAM** > **Roles** > **Create role**.
2. Select **AWS Service** and **Lambda**. Click **Next**.
3. Attach these three policies:
* `AmazonSQSFullAccess`
* `AmazonSNSFullAccess`
* `AWSLambdaBasicExecutionRole`
4. **Role name:** `lambda_execution_role`. Click **Create role**.

![1](https://github.com/user-attachments/assets/f52c6419-e068-4c0e-98f5-939b4869c4cb)

### **Step 2: Create the SQS Queues**

1. Navigate to **SQS** > **Create queue**.
2. **Name:** `nautilus-High-Priority-Queue`. Keep all defaults. Click **Create**.
3. **Create another queue.** Name: `nautilus-Low-Priority-Queue`. Click **Create**.
* *Copy both Queue URLs and ARNs to a notepad; you'll need them.*

![2](https://github.com/user-attachments/assets/0a3b109e-84f9-4398-99c6-28eaa2d87d64)

### **Step 3: Create the SNS Topic and Routing**

1. Navigate to **SNS** > **Topics** > **Create topic**.
2. Select **Standard**. **Name:** `nautilus-Priority-Queues-Topic`. Click **Create**.

![3](https://github.com/user-attachments/assets/b9941510-e3d6-42af-95e0-a101bb6fc3f5)

3. **High Priority Subscription:** Click **Create subscription**.
* **Protocol:** SQS.
* **Endpoint:** Select the `nautilus-High-Priority-Queue` ARN.
* **Subscription filter policy:** Expand this and paste:
```json
{ "priority": ["high"] }

```
* Click **Create subscription**.
  
![4](https://github.com/user-attachments/assets/7db56b3f-b706-4d64-94a0-432823346070)

4. **Low Priority Subscription:** Click **Create subscription**.
* **Endpoint:** Select the `nautilus-Low-Priority-Queue` ARN.
* **Subscription filter policy:** Paste:
```json
{ "priority": ["low"] }

```

* Click **Create subscription**.

![5](https://github.com/user-attachments/assets/fa7514fe-fc29-4fbe-890e-1df2e13ad216)

---

### **Step 4: Create and Configure the Lambda**

1. Navigate to **Lambda** > **Create function**.
2. **Name:** `nautilus-priorities-queue-function`. **Runtime:** Python 3.9.
3. **Permissions:** Select **Use an existing role** and choose `lambda_execution_role`. Click **Create**.
4. **Code:** Copy the content of `/root/index.py` from your host and paste it into the code editor. Click **Deploy**.
5. **Environment Variables:** Go to **Configuration** > **Environment variables** > **Edit**.
* Add `high_priority_queue` = `[Your High Priority Queue URL]`
* Add `low_priority_queue` = `[Your Low Priority Queue URL]`
* Click **Save**.
6. **General Configuration:** Edit the **Timeout** to **30 seconds**. Click **Save**.
7. **Triggers:** Click **+ Add trigger**. Select **SQS** and pick `nautilus-High-Priority-Queue`. **Repeat** for the Low Priority queue.

![6](https://github.com/user-attachments/assets/85c494e9-61fb-4056-94da-e89bea730e38)

---

### **Step 5: Testing (Via CLI)**

Run the publish commands from your host to ensure the high-priority messages are routed correctly:

```bash
topicarn=$(aws sns list-topics --query "Topics[?contains(TopicArn, 'nautilus-Priority-Queues-Topic')].TopicArn" --output text)

# Publish High then Low
aws sns publish --topic-arn $topicarn --message 'High Priority 1' --message-attributes '{"priority" : { "DataType":"String", "StringValue":"high"}}'
aws sns publish --topic-arn $topicarn --message 'Low Priority 1' --message-attributes '{"priority" : { "DataType":"String", "StringValue":"low"}}'

```

![7](https://github.com/user-attachments/assets/18f63bad-c405-4d03-a773-890807eae01d)

### **Step 6: Final Verification**

1. Check the **SQS Console**: The "Messages Available" should drop to 0 almost immediately.
2. Check **CloudWatch Logs** for the Lambda: Verify the "High Priority" message was processed.

👉[Dev.to](https://dev.to/hritikraj8804/aws-147-infrastructure-as-code-priority-queuing-with-sns-sqs-and-cloudformation-4eko)
