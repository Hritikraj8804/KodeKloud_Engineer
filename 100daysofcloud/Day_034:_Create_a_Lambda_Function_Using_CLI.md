# Instruction

The Nautilus DevOps team continues to explore serverless architecture by setting up another Lambda function. This time, the task must be completed using the AWS Console to familiarize the team with the web interface. The function will return a custom greeting and demonstrate the capabilities of AWS Lambda effectively.

Create Python Script: Create a Python script named `lambda_function.py` with a function that returns the body Welcome to KKE AWS Labs! and status code `200`.

Zip the Python Script: Zip the script into a file named `function.zip`.

Create Lambda Function: Create a Lambda function named `devops-lambda-cli` using the zipped file and specify Python as the runtime.

IAM Role: Use the IAM role named `lambda_execution_role`.

Use AWS CLI which is already configured on the `aws-client` host.

# Solutiom

### **Step 1: Create the Python Script**

First, create the function logic in a file named `lambda_function.py`.

```bash
cat <<EOF > lambda_function.py
import json

def lambda_handler(event, context):
    return {
        'statusCode': 200,
        'body': json.dumps('Welcome to KKE AWS Labs!')
    }
EOF

```

---

### **Step 2: Package the Code**

AWS Lambda requires your deployment package to be a `.zip` file when uploading via the CLI.

```bash
zip function.zip lambda_function.py

```

---

### **Step 3: Identify the Role ARN**

You'll need the Amazon Resource Name (ARN) of the existing `lambda_execution_role`. If you don't have it handy, you can retrieve it with this command:

```bash
ROLE_ARN=$(aws iam get-role --role-name lambda_execution_role --query 'Role.Arn' --output text)
echo $ROLE_ARN

```

![1 5](https://github.com/user-attachments/assets/8e67d4dc-e49e-477d-ae5c-2fe957cca3ec)

---

### **Step 4: Create the Lambda Function**

Now, use the `create-function` command. Note the use of `fileb://` for the zip file; this tells the CLI to treat the file as a binary blob.

```bash
aws lambda create-function \
    --function-name devops-lambda-cli \
    --runtime python3.12 \
    --role $ROLE_ARN \
    --handler lambda_function.lambda_handler \
    --zip-file fileb://function.zip

```

![1](https://github.com/user-attachments/assets/6f2f46da-51f8-4a32-a63f-f26c3fb56f8d)

---

### **Step 5: Verification**

To confirm the function was created and works as expected, invoke it directly from the CLI and check the output.

1. **Invoke the function**:
```bash
aws lambda invoke --function-name devops-lambda-cli output.json

```

![2](https://github.com/user-attachments/assets/796b38db-db30-473c-94c5-5eea84111017)

2. **Inspect the response**:
```bash
cat output.json

```

* **Expected Output**: `{"statusCode": 200, "body": "\"Welcome to KKE AWS Labs!\""}`

---

### **Why use the CLI for Lambda?**

* **Version Control**: You can track changes to your deployment scripts in Git.
* **Efficiency**: Updating the code is as simple as running `aws lambda update-function-code` rather than clicking through multiple console screens.
* **Infrastructure as Code (IaC)**: This CLI approach is the precursor to using tools like Terraform or AWS CloudFormation.

👉[Dev.to](https://dev.to/hritikraj8804/aws-134-cli-command-center-deploying-lambda-via-zipped-packages-4204)
