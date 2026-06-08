# Instruction

The Nautilus DevOps team has been creating several services on AWS Cloud and is continuously optimizing access management as part of their migration process.

For this task:

* An IAM user named `iamuser_kirsty` already exists
* An IAM policy named `iampolicy_kirsty` already exists
* Attach the IAM policy to the IAM user using Terraform
* Update the existing `main.tf` file only
* Do not create any additional `.tf` files
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

```bash
cd /home/bob/terraform
```



### **2. Open the Existing `main.tf` File**

```bash
vi main.tf
```

OR

```bash
nano main.tf
```



### **3. Update the Terraform Configuration**

Add the following configuration to attach the existing IAM policy to the existing IAM user:

```hcl
provider "aws" {
  region = "us-east-1"
}

data "aws_iam_user" "kirsty" {
  user_name = "iamuser_kirsty"
}

data "aws_iam_policy" "kirsty" {
  name = "iampolicy_kirsty"
}

resource "aws_iam_user_policy_attachment" "kirsty_attachment" {
  user       = data.aws_iam_user.kirsty.user_name
  policy_arn = data.aws_iam_policy.kirsty.arn
}
```

<img width="942" height="737" alt="config" src="https://github.com/user-attachments/assets/4d551d11-9f4c-4798-801b-d11ce6f214be" />

### **4. Initialize Terraform**

```bash
terraform init
```



### **5. Validate the Configuration**

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```



### **6. Apply the Changes**

```bash
terraform apply -auto-approve
```

Terraform will:

* Locate the existing IAM user
* Locate the existing IAM policy
* Attach the policy to the user



### **7. Verify the Policy Attachment**

Verify using AWS CLI:

```bash
aws iam list-attached-user-policies \
  --user-name iamuser_kirsty
```

Expected output should include:

```text
iampolicy_kirsty
```

Or verify from the AWS Console:

* **IAM**
* **Users**
* Select:

```text
iamuser_kirsty
```

* Open the **Permissions** tab

You should see:

```text
iampolicy_kirsty
```

listed under attached policies.

<img width="762" height="287" alt="verify" src="https://github.com/user-attachments/assets/e35ec80c-a315-41f2-9992-668d2d3a52cc" />


## Fun Message

*"Your IAM user now has the permissions it needs—Terraform just connected identity with access 🔐☁️🚀"*
