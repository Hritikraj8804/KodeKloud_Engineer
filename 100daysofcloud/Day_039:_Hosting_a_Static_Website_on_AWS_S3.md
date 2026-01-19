# Instruction

The Nautilus DevOps team has been tasked with creating an internal information portal for public access. As part of this project, they need to host a static website on AWS using an S3 bucket. The S3 bucket must be configured for public access to allow external users to access the static website directly via the S3 website URL.
Task Requirements:

Create an S3 bucket named `nautilus-web-14837`.

Configure the S3 bucket for static website hosting with `index.html` as the index document.

Allow public access to the bucket so that the website is publicly accessible.

Upload the `index.html` file from the `/root/` directory of the AWS client host to the S3 bucket.

Verify that the website is accessible directly through the S3 website URL.

# Solution


### **Step 1: Create the S3 Bucket**

1. **Navigate** to the S3 Console and click **Create bucket**.
2. **Bucket name**: `nautilus-web-14837`.
3. **Region**: Select the region closest to your users.
4. **Block Public Access settings**:
* **Uncheck** the box "Block all public access."
* Acknowledge the warning that the bucket will become public. (This is required for public website hosting).
5. Click **Create bucket**.

![1](https://github.com/user-attachments/assets/7b2d9335-3e35-4f9f-98ff-5ba14787644d)

---

### **Step 2: Configure Static Website Hosting**

1. Open your new bucket and go to the **Properties** tab.
2. Scroll to the bottom to find **Static website hosting** and click **Edit**.
3. Select **Enable**.
4. **Hosting type**: Host a static website.
5. **Index document**: Type `index.html`.
6. Click **Save changes**.
* *Note: Take note of the **Bucket website endpoint** URL that appears at the bottom of this section.*

![2](https://github.com/user-attachments/assets/ef3f8556-c13b-4752-b49e-8f23afaf93c6)

---

### **Step 3: Set Public Permissions (Bucket Policy)**

Even though you "unblocked" public access in Step 1, you still need to explicitly grant read permissions to the public via a policy.

1. Go to the **Permissions** tab and click **Edit** under **Bucket policy**.
2. Paste the following JSON (replace `nautilus-web-14837` if needed):
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::nautilus-web-14837/*"
        }
    ]
}

```


3. Click **Save changes**.

---

### **Step 4: Upload the Website Content**

On the `aws-client` host, use the CLI to upload the file to your new bucket.

```bash
aws s3 cp /root/index.html s3://nautilus-web-14837/

```

![3](https://github.com/user-attachments/assets/698cdcd9-65d7-4618-9189-92ebb0f1ef0c)

*Alternatively, you can use the **Upload** button in the S3 console and drag the file from your local machine.*

---

### **Step 5: Verify Accessibility**

1. Go back to the **Properties** tab of your bucket.
2. Scroll down to the **Static website hosting** section.
3. Click the link provided under **Bucket website endpoint**.
* The URL format will be: `http://nautilus-web-14837.s3-website-<region>.amazonaws.com`
4. Your internal information portal should now load in your browser.

![4](https://github.com/user-attachments/assets/a8a31d50-3e1f-44fc-8b52-dcbdace6a3ed)

---

### **Why use S3 for Hosting?**

* **Scalability**: S3 handles virtually unlimited traffic without any manual configuration.
* **Cost**: You only pay for the storage used and the data transferred out.
* **Security**: By using a restricted `s3:GetObject` policy, users can view the site but cannot modify or delete your files.

👉[Dev.to](https://dev.to/hritikraj8804/aws-139-serverless-hosting-static-websites-with-amazon-s3-12gi)
