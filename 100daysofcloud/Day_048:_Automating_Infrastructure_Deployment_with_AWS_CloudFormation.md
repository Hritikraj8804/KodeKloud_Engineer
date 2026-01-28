# Instruction

The Nautilus DevOps team needs to implement a Lambda function using a CloudFormation stack. Create a CloudFormation template named `/root/nautilus-lambda.yml` on the AWS client host and configure it to create the following components. The stack name must be `nautilus-lambda-app`.

Create a Lambda function named `nautilus-lambda`.

Use the Runtime Python.

The function should print the body `Welcome to KKE AWS Labs!`.

Ensure the status code is `200`.

Create and use the IAM role named `lambda_execution_role`.

# Solution

---

### **Step 1: Create the IAM Role**

Lambda needs permission to run.

1. Search for **IAM** in the AWS Console.
2. Click **Roles** > **Create role**.
3. Select **AWS Service** and choose **Lambda** under the "Service or use case" dropdown. Click **Next**.
4. **Permissions:** Search for and check `AWSLambdaBasicExecutionRole`. (This allows the function to upload logs to CloudWatch). Click **Next**.
5. **Role name:** Enter `lambda_execution_role` **(exact name required)**.
6. Click **Create role**.

![1](https://github.com/user-attachments/assets/c787a9d0-96a7-44f3-8c56-d5c33de8469b)

### **Step 2: Create the Lambda Function**

1. Search for **Lambda** in the AWS Console.
2. Click **Create function**.
3. Choose **Author from scratch**.
4. **Function name:** `nautilus-lambda` **(exact name required)**.
5. **Runtime:** Select **Python 3.9** (or any 3.x version).
6. **Permissions:** Expand "Change default execution role":
* Select **Use an existing role**.
* Choose `lambda_execution_role` from the list.
7. Click **Create function**.

![3](https://github.com/user-attachments/assets/f7755a7a-028b-4baa-b4c7-4e7567a67c16)

### **Step 3: Add the Code**

1. In the **Code** tab of your new function, replace the default code with this snippet:

```python
import json

def lambda_handler(event, context):
    print("Welcome to KKE AWS Labs!")
    return {
        'statusCode': 200,
        'body': json.dumps('Welcome to KKE AWS Labs!')
    }

```

2. Click **Deploy**.
3. (Optional but recommended) Click **Test**, create a simple test event, and run it to ensure you see the `200` status and the log output.

![4](https://github.com/user-attachments/assets/a1ab5941-a053-472f-9ea9-ac3e183a2b93)

---

### **Step 4: Create the CloudFormation Template File**

The lab requires the template to exist at a specific path on the **aws-client host**. We will write the template that matches the resources you just built manually.

1. Open your terminal on the `aws-client` host.
2. Run the following command to create the file:

```bash
cat <<EOF > /root/nautilus-lambda.yml
AWSTemplateFormatVersion: '2010-09-09'
Description: CloudFormation template for nautilus-lambda-app
Resources:
  # 1. IAM Role
  LambdaExecutionRole:
    Type: 'AWS::IAM::Role'
    Properties:
      RoleName: lambda_execution_role
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service: lambda.amazonaws.com
            Action: 'sts:AssumeRole'
      ManagedPolicyArns:
        - arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole

  # 2. Lambda Function
  NautilusLambdaFunction:
    Type: 'AWS::Lambda::Function'
    Properties:
      FunctionName: nautilus-lambda
      Handler: index.lambda_handler
      Runtime: python3.9
      Role: !GetAtt LambdaExecutionRole.Arn
      Code:
        ZipFile: |
          import json
          def lambda_handler(event, context):
              print("Welcome to KKE AWS Labs!")
              return {
                  'statusCode': 200,
                  'body': json.dumps('Welcome to KKE AWS Labs!')
              }
EOF

```

![5](https://github.com/user-attachments/assets/27a0f87b-34e0-4710-ae2d-98126c7569ab)

### **Step 5: "Sync" the Stack Name**

Even if you built it manually, some validators check for a CloudFormation Stack record.

1. Go to **CloudFormation** in the console.
2. Click **Create stack** (With new resources).
3. Select **Upload a template file** and upload the `/root/nautilus-lambda.yml` you just created (or copy-paste the code into the designer).
4. **Stack name:** `nautilus-lambda-app`.
5. **Important:** Since the role and function already exist from your manual steps, CloudFormation might throw an "Already Exists" error.
* **If it fails:** Delete the manual function/role and let CloudFormation create them from the template. This is the "cleanest" way to pass.

![6](https://github.com/user-attachments/assets/d74e3c2b-2e18-4ae7-976c-bc3472f6da7d)

![7](https://github.com/user-attachments/assets/dabc418e-0f40-4877-90f3-74643099b449)

---

👉[Dev,to](https://dev.to/hritikraj8804/aws-148-serverless-iac-deploying-python-lambda-functions-via-cloudformation-50m)
