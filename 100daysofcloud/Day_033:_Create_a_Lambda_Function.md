# Instruction

The Nautilus DevOps team is embracing serverless architecture by integrating AWS Lambda into their operational tasks. They have decided to deploy a simple Lambda function that will return a custom greeting to demonstrate serverless capabilities effectively. This function is crucial for showcasing rapid deployment and easy scalability features of AWS Lambda to the team.

Create Lambda Function: Create a Lambda function named `devops-lambda`.

Runtime: Use the Runtime Python.

Deploy: The function should print the body `Welcome to KKE AWS Labs!`.

Status Code: Ensure the status code is `200`.

IAM Role: Create and use the IAM role named `lambda_execution_role`.

Use the AWS Console to complete this task.

# Solution


### **Step 1: Create the IAM Execution Role**

Before creating the function, you must ensure the execution role exists so Lambda has permission to run and log to CloudWatch.

1. Navigate to the **IAM Console** > **Roles** > **Create role**.
2. **Select trusted entity**: Choose **AWS service** and then select **Lambda**. Click **Next**.
3. **Add permissions**: Search for and check the box for `AWSLambdaBasicExecutionRole` (this allows the function to upload logs to CloudWatch). Click **Next**.
4. **Name, review, and create**:
* **Role name**: `lambda_execution_role`.


5. Click **Create role**.

---

### **Step 2: Create the Lambda Function**

1. Navigate to the **Lambda Console** and click **Create function**.
2. Choose **Author from scratch**.
3. **Basic information**:
* **Function name**: `devops-lambda`.
* **Runtime**: Select **Python** (e.g., Python 3.12).


4. **Permissions**:
* Expand **Change default execution role**.
* Select **Use an existing role**.
* Choose `lambda_execution_role` from the dropdown.


5. Click **Create function**.

---

### **Step 3: Configure and Deploy the Code**

1. In the **Code** tab, locate the `lambda_function.py` file in the code editor.
2. Replace the default code with the following snippet to meet the requirements:
```python
import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Welcome to KKE AWS Labs!')
    }

```


3. Click the **Deploy** button above the code editor to save your changes.

---

### **Step 4: Verification (Test the Function)**

To ensure the function returns the correct greeting and status code:

1. Click the **Test** button.
2. Create a new test event (name it `TestGreeting`). You can leave the default JSON as is.
3. Click **Save**, then click **Test** again.
4. Check the **Execution results**:
* **Status**: Succeeded.
* **Response body**: `"Welcome to KKE AWS Labs!"`.
* **Status Code**: `200`.

![1](https://github.com/user-attachments/assets/ce020e5b-4baa-4ebd-8f23-d670f2090ec4)

---

### **Key Serverless Benefits for the Team**

* **Zero Administration**: No servers to patch or manage; AWS handles the underlying infrastructure.
* **Pay-per-Execution**: You only pay for the milliseconds the code is actually running.
* **Automatic Scaling**: Whether the team triggers this once a day or 1,000 times a second, Lambda scales automatically.

👉[Dev.to](https://dev.to/hritikraj8804/aws-133-going-serverless-deploying-your-first-aws-lambda-function-55ja)
