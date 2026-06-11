# Instruction

During the migration process, several resources were created under the AWS account. Some of these test resources are no longer needed at the moment, so they need to be cleaned up temporarily.

For this task:

* Delete the EC2 instance named `devops-ec2`
* Region: `us-east-1`
* Use Terraform
* Keep the provisioning code in `main.tf`
* Ensure the instance reaches the **terminated** state before submitting
* Terraform working directory: `/home/bob/terraform`



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

```bash
cd /home/bob/terraform
```



### **2. Verify the Instance Exists**

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=devops-ec2"
```



### **3. Review Terraform Resources**

Check what Terraform manages:

```bash
terraform state list
```

Ensure the EC2 instance appears in the state file.

<img width="772" height="322" alt="main" src="https://github.com/user-attachments/assets/22196ba7-2434-4d5c-8565-1e22614d11d9" />

### **4. Destroy the EC2 Instance**

Since the task requires keeping the Terraform code for future provisioning, destroy only the resource:

```bash
terraform destroy -auto-approve
```

If multiple resources exist and only the EC2 instance should be deleted:

```bash
terraform destroy -target=aws_instance.devops_ec2 -auto-approve
```

Replace `aws_instance.devops_ec2` with the actual resource name from `main.tf`.

<img width="577" height="117" alt="destoryed" src="https://github.com/user-attachments/assets/8f50fcf5-6d84-4b0f-911b-445a0708fc9e" />

### **5. Verify the Instance is Terminated**

Check the instance state:

```bash
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=devops-ec2"
```

Expected state:

```text
terminated
```

You can also verify from the AWS Console:

* EC2 Dashboard
* Instances
* Search for:

```text
devops-ec2
```

The state should be:

```text
terminated
```

<img width="647" height="122" alt="term" src="https://github.com/user-attachments/assets/4c48f8fa-94e1-456c-b4f1-c582a6c2540c" />

### **6. Keep the Terraform Code**

Do **not** delete the EC2 resource block from `main.tf`.

The code should remain so the instance can be recreated later with:

```bash
terraform apply
```



## Challenges

### **1. Don't Delete the Code**

The task explicitly says:

```text
Keep the provisioning code
```

Only destroy the resource, not the Terraform configuration.



### **2. Wait for Full Termination**

AWS may show:

```text
shutting-down
```

for a short time before reaching:

```text
terminated
```

Wait until termination is complete before submitting.



### **3. Verify Before Submission**

Run:

```bash
aws ec2 describe-instances
```

and confirm the final state is:

```text
terminated
```



## Fun Message

*"Infrastructure as Code works both ways—today Terraform helped you clean up the cloud instead of building it 🧹☁️🚀"*
