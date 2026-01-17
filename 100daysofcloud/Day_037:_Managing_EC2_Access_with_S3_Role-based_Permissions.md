# Instruction

The Nautilus DevOps team needs to set up an application on an EC2 instance to interact with an S3 bucket for storing and retrieving data. To achieve this, the team must create a private S3 bucket, set appropriate IAM policies and roles, and test the application functionality.

Task:

1) EC2 Instance Setup:

An instance named `datacenter-ec2` already exists.

The instance requires access to an S3 bucket.

2) Setup SSH Keys:

Create new SSH key pair (`id_rsa` and `id_rsa.pub`) on the `aws-client` host and add the public key to the root user's authorized keys on the EC2 instance.

3) Create a Private S3 Bucket:

Name the bucket `datacenter-s3-15606`.

Ensure the bucket is private.

4) Create an IAM Policy and Role:

Create an IAM policy allowing `s3:PutObject`, `s3:ListBucket` and `s3:GetObject` access to `datacenter-s3-15606`.

Create an IAM role named `datacenter-role`.

Attach the policy to the IAM role.

Attach this role to the `datacenter-ec2` instance.

5) Test the Access:

SSH into the EC2 instance and try to upload a file to datacenter-s3-15606 bucket using following command:

`aws s3 cp <your-file> s3://datacenter-s3-15606/`

Now run following command to list the upload file:

`aws s3 ls s3://datacenter-s3-15606/`

# Solution

  <img width="1014" height="2890" alt="10" src="https://github.com/user-attachments/assets/75bf3bc2-67d8-425e-bbfe-a962a1431a80" />

  
### **Step 1: Setup SSH Key Access**

On the `aws-client` host, generate a new key pair to enable password-less access to the `datacenter-ec2` instance.

1. **Generate Keys**: Run `ssh-keygen -t rsa -f ~/.ssh/id_rsa -N ""` to create `id_rsa` and `id_rsa.pub`.
  ![1](https://github.com/user-attachments/assets/7b6ade48-485d-443e-a4d3-1ed79d85d390)

2. **Authorize Key**: Copy the content of `~/.ssh/id_rsa.pub` and append it to the `/root/.ssh/authorized_keys` file on the `datacenter-ec2` instance.
* *Note: Ensure permissions are set correctly on the instance: `chmod 700 ~/.ssh` and `chmod 600 ~/.ssh/authorized_keys`.*
---

### **Step 2: Create a Private S3 Bucket**

Create the storage container with restricted access.

1. **Navigate** to S3 in the AWS Console.
2. **Create Bucket**: Name it `datacenter-s3-15606`.
3. **Permissions**: Ensure "Block all public access" is **checked** (default). This keeps the bucket private.

    ![3](https://github.com/user-attachments/assets/d742629b-f240-4572-ba10-003a3ce3e03d)

---

### **Step 3: IAM Policy and Role Configuration**

This step creates the "permission bridge" between the server and the bucket.

#### **Create the IAM Policy**

1. Go to **IAM > Policies > Create Policy**.
2. Select the **JSON** tab and paste the following (replacing `BUCKET_NAME` with `datacenter-s3-15606`):
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject"
            ],
            "Resource": "arn:aws:s3:::datacenter-s3-15606/*"
        },
        {
            "Effect": "Allow",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::datacenter-s3-15606"
        }
    ]
}
```
  ![4](https://github.com/user-attachments/assets/051cfbf8-b285-40eb-b65c-bd5549849c3c)

3. Name the policy (e.g., `S3AccessPolicy-15606`) and create it.

#### **Create and Attach the IAM Role**

1. Go to **IAM > Roles > Create Role**.
2. **Trusted Entity**: Select **AWS Service** and choose **EC2**.
3. **Permissions**: Search for and attach your newly created `S3AccessPolicy-15606`.
4. **Role Name**: `datacenter-role`. Create the role.
  ![5](https://github.com/user-attachments/assets/ed3a1db2-b6d3-4256-8e56-dabcf93c220d)
5. **Attach to Instance**: Go to the **EC2 Console**, select `datacenter-ec2` > **Actions** > **Security** > **Modify IAM Role**. Select `datacenter-role` and save.

  ![6](https://github.com/user-attachments/assets/15c9cd2c-f0f6-4566-ab5b-ab3ed501b3e3)

---

### **Step 4: Verification and Testing**

Finally, log into the instance and test the new permissions.

1. **SSH into EC2**: From the client, run `ssh root@<datacenter-ec2-ip>`.

  ![7](https://github.com/user-attachments/assets/ea4f6835-2273-4b5a-971d-620b97f2aa35)

3. **Upload Test**: Create a small file and push it to S3.
```bash
echo "Nautilus Test File" > testfile.txt
aws s3 cp testfile.txt s3://datacenter-s3-15606/
```
3. **List Objects**: Verify the upload was successful.
```bash
aws s3 ls s3://datacenter-s3-15606/
```

  ![8](https://github.com/user-attachments/assets/665eadbd-c4a1-45fb-84c6-671f92a9de42)

  ![9](https://github.com/user-attachments/assets/2f507020-4d46-4df9-962e-2a25eebcf390)

---

### **Why use an IAM Role?**

Using a role (Instance Profile) means the EC2 instance retrieves temporary security credentials automatically. You never have to run `aws configure` or store secret keys on the server, which significantly reduces the risk of credential leakage.

👉[Dev.to](https://dev.to/hritikraj8804/aws-137-secure-resource-access-iam-roles-for-ec2-and-s3-5hg3)
