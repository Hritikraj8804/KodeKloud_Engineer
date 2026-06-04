# Instruction

The Nautilus DevOps team needs to set up an Amazon OpenSearch Service domain to store and search application logs. OpenSearch provides scalable search, analytics, and log management capabilities.

For this task:

* Create an OpenSearch domain named `nautilus-es`
* Use Terraform
* Terraform working directory: `/home/bob/terraform`
* Create only the `main.tf` file
* Before submitting the task, ensure:

```bash
terraform plan
```

returns:

```text
No changes. Your infrastructure matches the configuration.
```

* Wait until the OpenSearch domain status becomes active before submitting

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

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
provider "aws" {
  region = "us-east-1"
}

resource "aws_opensearch_domain" "nautilus_es" {
  domain_name    = "nautilus-es"
  engine_version = "OpenSearch_2.3"

  cluster_config {
    instance_type = "t3.small.search"
  }

  ebs_options {
    ebs_enabled = true
    volume_size = 10
  }
}
```

<img width="917" height="451" alt="main" src="https://github.com/user-attachments/assets/db812cb7-5b59-45a4-98b2-52eec1c03801" />


### **4. Initialize Terraform**

Initialize Terraform and download the required AWS provider plugins:

```bash
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the Terraform configuration:

```bash
terraform validate
```

Expected output:

```bash
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the OpenSearch domain:

```bash
terraform apply -auto-approve
```

Terraform will:

* Create the OpenSearch domain
* Configure the search cluster
* Provision attached EBS storage

> **Note:** OpenSearch domain creation can take **10–20 minutes** or longer.



### **7. Verify the OpenSearch Domain**

Verify the domain using AWS CLI:

```bash
aws opensearch describe-domain \
  --domain-name nautilus-es
```

Look for:

```text
DomainName : nautilus-es
Processing : false
```

When `Processing` becomes `false`, the domain is fully created and ready for use.

Or verify from the AWS Console:

* **Amazon OpenSearch Service**
* **Domains**
* Search for:

```text
nautilus-es
```

Status should show:

```text
Active
```



### **8. Verify Terraform State**

Before submitting the task, run:

```bash
terraform plan
```

Expected output:

```text
No changes. Your infrastructure matches the configuration.
```

This confirms that the deployed infrastructure matches the Terraform configuration exactly.

<img width="742" height="383" alt="verify" src="https://github.com/user-attachments/assets/74457ab9-91c6-4d47-ab85-1f2a612ebff0" />


## Terraform Resources Used

| Resource                | Purpose                                     |
| ----------------------- | ------------------------------------------- |
| `aws_opensearch_domain` | Creates an Amazon OpenSearch Service domain |


## Fun Message

*"Your search engine is now live in the cloud—ready to index logs, power analytics, and answer queries at lightning speed 🔍⚡☁️"*
