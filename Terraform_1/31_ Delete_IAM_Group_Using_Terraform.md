# Instruction

The Nautilus DevOps team is currently engaged in a cleanup process, focusing on removing unnecessary resources from their AWS account. As part of this effort, an IAM group is no longer required and must be deleted.

For this task:

* Delete the IAM group named `iamgroup_rose`
* Use Terraform
* Keep the provisioning code intact for future use
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

```bash
cd /home/bob/terraform
```



### **2. Verify the IAM Group Exists**

```bash
aws iam get-group --group-name iamgroup_rose
```

You should see details of the group if it exists.

<img width="791" height="202" alt="config" src="https://github.com/user-attachments/assets/4d02c9ad-d665-4322-b19d-eb8e534d2562" />

### **3. Review the Terraform Configuration**

Open the existing `main.tf` file:

```bash
vi main.tf
```

OR

```bash
nano main.tf
```

Verify that the IAM group resource definition is still present. **Do not remove it**, as the task explicitly requires keeping the provisioning code.

Example:

```hcl
resource "aws_iam_group" "iamgroup_rose" {
  name = "iamgroup_rose"
}
```



### **4. Initialize Terraform (If Required)**

```bash
terraform init
```



### **5. Destroy the IAM Group**

Since KodeKloud labs for these cleanup tasks typically expect the resource to be destroyed while keeping the code unchanged, run:

```bash
terraform destroy -auto-approve
```

This removes the IAM group managed by the Terraform state while leaving the code in `main.tf`.

<img width="750" height="431" alt="destory" src="https://github.com/user-attachments/assets/f7074c87-f582-49b4-9970-45195e3e3dd0" />


### **6. Verify the IAM Group is Deleted**

Run:

```bash
aws iam get-group --group-name iamgroup_rose
```

Expected result:

```text
NoSuchEntity
```

Or verify from the AWS Console:

* IAM Dashboard
* User Groups
* Search for:

```text
iamgroup_rose
```

The group should no longer exist.



### **7. Confirm Terraform Cleanup**

You can verify the state is empty:

```bash
terraform state list
```

Expected output:

```text
No resources found
```


## Fun Message

*"Infrastructure as Code isn't just about building resources—today Terraform helped you clean up the cloud with a single command 🧹☁️🚀"*
