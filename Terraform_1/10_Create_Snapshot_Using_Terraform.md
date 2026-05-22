# Instruction

The Nautilus DevOps team has some volumes in different regions in their AWS account. They are going to set up automated backups so that all important data can be backed up regularly. For this task, they need to create a snapshot of an existing EBS volume.

For this task:

* Create a snapshot of the existing EBS volume named `devops-vol`
* Region must be `us-east-1`
* Snapshot name must be `devops-vol-ss`
* Snapshot description must be `Devops Snapshot`
* Ensure the snapshot state becomes `completed`
* Update the existing `main.tf` file only
* Terraform working directory: `/home/bob/terraform`

> **Note:** Right-click under the EXPLORER section in VS Code and select **Open in Integrated Terminal** to launch the terminal.



# Solution

## **Given Existing `main.tf`**

```hcl id="q8w2rm"
resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name = "devops-vol"
  }
}
```



## **Steps to Complete the Task**

### **1. Navigate to the Terraform Working Directory**

Open the integrated terminal and move to the Terraform directory:

```bash id="k4n7xp"
cd /home/bob/terraform
```



### **2. Open the Existing `main.tf` File**

Edit the existing Terraform configuration file:

```bash id="m9v3qt"
vi main.tf
```

OR

```bash id="r5x8ld"
nano main.tf
```



### **3. Update the Terraform Configuration**

Replace the existing configuration with the following:

```hcl id="u2p6fk"
provider "aws" {
  region = "us-east-1"
}

resource "aws_ebs_volume" "k8s_volume" {
  availability_zone = "us-east-1a"
  size              = 5
  type              = "gp2"

  tags = {
    Name = "devops-vol"
  }
}

resource "aws_ebs_snapshot" "devops_snapshot" {
  volume_id   = aws_ebs_volume.k8s_volume.id
  description = "Devops Snapshot"

  tags = {
    Name = "devops-vol-ss"
  }

  depends_on = [
    aws_ebs_volume.k8s_volume
  ]
}
```

<img width="742" height="423" alt="main" src="https://github.com/user-attachments/assets/1f48f1bb-7f3d-43cd-a9b6-e51f5c7cd083" />


### **4. Initialize Terraform**

Initialize Terraform and download the required providers:

```bash id="t7m1wc"
terraform init
```



### **5. Validate the Terraform Configuration**

Validate the configuration before deployment:

```bash id="y3q8vn"
terraform validate
```

Expected output:

```bash id="f6r2ks"
Success! The configuration is valid.
```



### **6. Apply the Terraform Configuration**

Create the EBS volume and snapshot using:

```bash id="j5n9pd"
terraform apply -auto-approve
```

Terraform will:

* Create the EBS volume
* Create the snapshot
* Wait until the snapshot process completes



### **7. Verify the Snapshot**

Verify the snapshot using AWS CLI:

```bash id="x8v4tm"
aws ec2 describe-snapshots --owner-ids self
```

Look for:

```text id="p2m7qs"
devops-vol-ss
```

and ensure the snapshot state shows:

```text id="d9k3wr"
completed
```

You can also verify from the AWS Console:

* **EC2 Dashboard**
* **Snapshots**
* Search for:

```text id="n4x1lb"
devops-vol-ss
```

<img width="652" height="453" alt="list" src="https://github.com/user-attachments/assets/da543b91-cf65-4d55-a2e4-be44d3a5d763" />


## Terraform Resources Used

| Resource           | Purpose                              |
| ------------------ | ------------------------------------ |
| `aws_ebs_volume`   | Creates the EBS volume               |
| `aws_ebs_snapshot` | Creates a snapshot of the EBS volume |



## Challenges

### **1. Snapshot Takes Time**

AWS snapshots may take a few moments before reaching the:

```text id="v7r5qc"
completed
```

state.



### **2. Incorrect Volume Reference**

Ensure the snapshot uses the correct volume ID:

```hcl id="h3m8zd"
volume_id = aws_ebs_volume.k8s_volume.id
```



### **3. Missing Tags**

The validation expects the snapshot name tag:

```text id="k6p2xf"
devops-vol-ss
```

and description:

```text id="w1n9ts"
Devops Snapshot
```



## Fun Message

*"Your cloud backup is now safely snapshotted — Terraform just gave your data a rewind button 📸💾☁️"*
