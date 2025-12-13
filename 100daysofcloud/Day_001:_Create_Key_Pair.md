# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.
For this task, create a key pair with the following requirements:
Name of the key pair should be devops-kp.

Key pair type must be rsa

Notes:
Create the resources only in us-east-1 region.

# Solution

### **Steps to Complete the Task:**

1. **Log in to the AWS Console**

   * Use the provided AWS credentials to log in
   * Make sure to do this within the provided time window

2. **Navigate to the EC2 Dashboard:**

   * From the AWS Console, go to the **EC2 Dashboard**.

3. **Create a Key Pair:**

   * In the left-hand sidebar, under **Network & Security**, select **Key Pairs**.
   * Click on the **Create key pair** button.

4. **Configure Key Pair Settings:**

   * **Key Pair Name:** Enter `devops-kp`.
   * **Key Pair Type:** Select `RSA`.
   * **Private Key File Format:** Choose **PEM** (for Linux/Mac) or **PPK** (for Windows).
   * Click **Create key pair**.

   **Note:** The private key file (e.g., `devops-kp.pem`) will automatically download to your computer. Save it securely.

5. **Verify the Key Pair:**

   * In the **Key Pairs** section, check that your newly created key pair (`devops-kp`) is listed.
  
     ![key_created](https://github.com/user-attachments/assets/3b3177fa-17a1-4cd9-94e1-e2b0f87955f9)


6. **Ensure you are in the Correct Region:**

   * Double-check that your region is set to **us-east-1** in the top right corner of the AWS Console to ensure the key pair is created in the correct region.

## Challenges:

* **Remember to save the private key file:** If you lose the `.pem` file, you won't be able to access your EC2 instances using SSH.
* **Region-specific resources:** Ensure you are in the `us-east-1` region when creating the key pair, as resources in AWS are region-specific.

## Fun Message:

*"Well, you’ve created your key pair. Congratulations, you now hold the key to the cloud literally!"*

👉[Dev.to](https://dev.to/hritikraj8804/aws-101-the-first-step-to-the-cloud-creating-your-essential-ssh-key-pair-5f1h)
