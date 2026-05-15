# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

For this task, create a key pair using Terraform with the following requirements:

* Name of the key pair should be `devops-kp`.
* Key pair type must be `rsa`.
* The private key file should be saved under `/home/bob/devops-kp.pem`.
* The Terraform working directory is `/home/bob/terraform`.
* Create the `main.tf` file (do not create a different `.tf` file) to accomplish this task.

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal in VS Code and move to the Terraform directory:

```bash
cd /home/bob/terraform
```



### **2. Create the `main.tf` File**

Create the Terraform configuration file:

```bash
vi main.tf
```

OR

```bash
nano main.tf
```



### **3. Add the Terraform Configuration**

Paste the following code into the `main.tf` file:

```hcl
resource "tls_private_key" "devops_key" {
  algorithm = "RSA"
  rsa_bits  = 2048
}

resource "aws_key_pair" "devops_kp" {
  key_name   = "devops-kp"
  public_key = tls_private_key.devops_key.public_key_openssh
}

resource "local_file" "private_key" {
  content  = tls_private_key.devops_key.private_key_pem
  filename = "/home/bob/devops-kp.pem"
}
```

<img width="960" height="507" alt="main" src="https://github.com/user-attachments/assets/1829d5e4-c717-46de-9757-97d74869b368" />



### **4. Initialize Terraform**

Run the following command to initialize Terraform and download the required providers:

```bash
terraform init
```



### **5. Validate the Configuration**

Check whether the configuration is valid:

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the resources using:

```bash
terraform apply
```

Type:

```bash
yes
```

when prompted.

<img width="770" height="472" alt="apply1" src="https://github.com/user-attachments/assets/961dce81-5524-4acb-96fd-154bbc2a88d7" />

<img width="952" height="167" alt="apply2" src="https://github.com/user-attachments/assets/f844ea99-8ea5-41d8-9a57-8e87834d5c39" />



### **7. Verify the Key Pair**

Check whether the key pair file exists:

```bash
ls -l /home/bob/devops-kp.pem
```

<img width="752" height="226" alt="pem" src="https://github.com/user-attachments/assets/ba5ef1b8-42ac-40ef-a760-569d9ee2bf07" />


You can also verify the key pair from the AWS Console under:

* **EC2 Dashboard**
* **Network & Security**
* **Key Pairs**

You should see:

```text
devops-kp
```



## Terraform Resources Used

| Resource          | Purpose                                  |
| ----------------- | ---------------------------------------- |
| `tls_private_key` | Generates an RSA private/public key pair |
| `aws_key_pair`    | Uploads the public key to AWS            |
| `local_file`      | Saves the private key locally            |



## Challenges

### **1. Forgetting Required Providers**

Terraform automatically downloads providers during `terraform init`, but internet access and proper provider configuration are necessary.



### **2. File Permission Issues**

Sometimes the `.pem` file may not have secure permissions. Fix it using:

```bash
chmod 400 /home/bob/devops-kp.pem
```



### **3. Wrong Working Directory**

Running Terraform commands outside `/home/bob/terraform` can create state files in unexpected locations.



## Fun Message

*"Terraform didn’t just create a key pair… it automated trust between you and the cloud 🚀"*
