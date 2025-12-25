# Instruction

During the migration process, several resources were created under the AWS account. Later on, some of these resources became obsolete as alternative solutions were implemented. Similarly, there is an instance that needs to be deleted as it is no longer in use.
1) Delete the ec2 instance named `xfusion-ec2` present in `us-east-1` region.
2) Before submitting your task, make sure instance is in terminated state.

# Solution

Follow these steps using the AWS Management Console to terminate the required instance:

#### Step 1: Navigate to the EC2 Dashboard

1. Log into the **AWS Management Console**.
2. Ensure your region is set to **us-east-1 (N. Virginia)**.

![1](https://github.com/user-attachments/assets/f28b4b9a-585c-472e-a095-ffcb8acb3a0c)

3. Search for **EC2** and select it to open the dashboard.

![2](https://github.com/user-attachments/assets/2c88673c-f8eb-483c-8d5d-b8b27613d28d)


#### Step 2: Access the Instances List

1. Click on **Instances**.

![3](https://github.com/user-attachments/assets/80b799b0-d43f-4af2-b3fb-a808b71d70fb)

2. Search for `xfusion-ec2` in the filter bar to locate the specific instance.

#### Step 3: Initiate Termination

1. Select the checkbox next to the **`xfusion-ec2`** instance.
2. Click the **Instance state** button at the top of the console.
3. Select **Terminate instance** from the dropdown menu.

![4](https://github.com/user-attachments/assets/ae58f675-d337-40d9-97cd-bdb9b84def23)

#### Step 4: Confirm Action

1. A confirmation dialog will appear warning you about the permanent nature of termination.
2. Click the **Terminate** button to confirm.

![5](https://github.com/user-attachments/assets/dbee4c0e-36a0-4dd9-a130-20cacacad78f)

#### Step 5: Verification

1. Observe the **Instance state** column for `xfusion-ec2`. It will first transition to `shutting-down`.
2. Wait for the status to change to **`terminated`**.
3. Refresh the page if necessary to update the state.

![6](https://github.com/user-attachments/assets/e8ee3503-c96c-4b5d-90b1-5157634b1ac5)

---

### Challenges

Here are potential issues or behaviors to watch out for:

* **Termination Protection:** If the task fails with an error, it is likely that "Termination Protection" is enabled. You must first disable this attribute via **Actions > Instance settings > Change termination protection** before you can delete the instance.
* **Auto Scaling Groups:** If the instance belongs to an Auto Scaling Group (ASG), the ASG may automatically launch a replacement instance immediately after you terminate this one. You would need to update the ASG capacity or delete the ASG itself.
* **Dangling EBS Volumes:** By default, the root volume is deleted upon termination, but any additional data volumes might persist and continue to incur charges if the "Delete on termination" checkbox was not checked during attachment.
* **Persistence in Console:** Terminated instances remain visible in the AWS Console for approximately 1–2 hours before disappearing completely. This is normal behavior.

---

### Fun Message

> And... poof! `xfusion-ec2` has officially left the building. It’s gone to that big data center in the sky where the uptime is 100% and the latency is zero. Don't worry, it didn't feel a thing!

👉[Dev.to](https://dev.to/hritikraj8804/aws-114-cleaning-up-the-cloud-how-to-terminate-an-ec2-instance-3fo6)
