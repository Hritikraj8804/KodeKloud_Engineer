# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.
For this task, create an AMI from an existing EC2 instance `named devops-ec2` with the following requirement:
Name of the AMI should be `devops-ec2-ami`, make sure AMI is in available state.

# Solution

Follow these steps to create the AMI and verify its availability:

#### Step 1: Locate the Source Instance

1. Log into the **AWS Management Console** and navigate to the **EC2 Dashboard**.

![1](https://github.com/user-attachments/assets/75671974-8dd6-4788-aa04-f087bde0deaf)

![2](https://github.com/user-attachments/assets/3e76cc8f-21c3-43d2-a82a-775311c6b1c3)

2. Ensure you are in the **us-east-1** region.
3. Click on **Instances** in the left-hand menu.
4. Find the instance named `devops-ec2` and ensure it is selected.

![3](https://github.com/user-attachments/assets/cbfeead2-8624-49cd-95d0-dd7490df7211)

#### Step 2: Initiate Image Creation

1. With `devops-ec2` selected, click the **Actions** button at the top.
2. Navigate to **Image and templates** and select **Create image**.

![4](https://github.com/user-attachments/assets/b1c9fad7-8e68-41bb-8473-ffc1a2ffa4bc)

#### Step 3: Configure AMI Settings

1. On the **Create image** page:
* **Image name:** Enter the required name: `devops-ec2-ami`.
* **Image description:** (Optional) You can add a description like "AMI for devops-ec2 migration".
* **No reboot:** Leave this **unchecked** (default) to ensure a consistent snapshot by allowing AWS to reboot the instance.

![5](https://github.com/user-attachments/assets/ebfa9c42-53e3-495b-b254-20a5a2a09d0d)

2. Review the instance's volumes (root and any attached EBS volumes) to ensure they are included in the image.
3. Click the **Create image** button at the bottom right.

#### Step 4: Monitor AMI Status

1. In the left navigation pane, under **Images**, click on **AMIs**.

![6](https://github.com/user-attachments/assets/ff2bfe14-c7c1-40ef-9bbe-f6459414d19f)

2. Locate your new AMI, `devops-ec2-ami`.
3. Observe the **Status** column. It will initially show as **`pending`**.

![7](https://github.com/user-attachments/assets/85213058-b7b9-4a99-95e7-13d126d1d1c5)


#### Step 5: Verification

1. Wait for the creation process to complete. This can take several minutes depending on the volume size.
2. Refresh the page until the status changes from `pending` to **`available`**.

![8](https://github.com/user-attachments/assets/96f128a6-1359-4cc9-8f9a-3b121d16ec05)

---

### Challenges

* **Long Wait Times:** Large volumes or high I/O activity on the source instance can cause the AMI creation to take 20 minutes or longer.
* **Instance Downtime:** Because "No reboot" is unchecked by default, the instance will be temporarily unavailable while it restarts. Plan this during a maintenance window if necessary.
* **Pending Snapshots:** If you see the AMI is `pending`, you can check the **Snapshots** section under **Elastic Block Store** to see the progress of the underlying data backup.
* **Region Specificity:** AMIs are regional. If you need this image in another region for disaster recovery, you must manually **Copy AMI** to that region after it becomes `available`.

---

### Fun Message

> Say cheese! Your instance just had its professional headshot taken. Now you have a perfect "digital clone" ready to be deployed whenever you need a twin. Just don't let the two instances start arguing over who's the original!

👉[Dev.to](https://dev.to/hritikraj8804/aws-113-capturing-the-moment-how-to-create-an-amazon-machine-image-ami-1cl7)
