# Instruction

As part of the migration, there were some components added to the AWS account. Team created one of the EC2 instances where they need to make some changes now.
There is an EC2 instance named `datacenter-ec2` under `us-east-1` region, enable the stop protection for this instance.

# Solution

Follow these steps to enable protection for the `datacenter-ec2` instance:

#### Step 1: Locate the Instance

1. Log into the **AWS Management Console**.
2. Ensure you are in the **us-east-1 (N. Virginia)** region.

![1](https://github.com/user-attachments/assets/41864f57-b5f7-46d3-b4ba-c5c4e485a68b)

3. Navigate to **EC2** > **Instances**.

![2](https://github.com/user-attachments/assets/0b4708d8-1eb8-43a3-83c0-49c824bb46c5)

![3](https://github.com/user-attachments/assets/59e9c105-f5f8-40e5-b06c-3d1b5c7b6844)

4. Find the instance named `datacenter-ec2` in the list.

![4](https://github.com/user-attachments/assets/b4fd4276-ee34-449f-ab7f-076a6090214b)

#### Step 2: Access Instance Settings

1. Select the checkbox next to the `datacenter-ec2` instance.
2. Click the **Actions** dropdown menu at the top.
3. Navigate to **Instance settings** and select **Change termination protection**.

![5](https://github.com/user-attachments/assets/560d779d-1e7b-483b-b98d-aaa36fa23b95)

#### Step 3: Enable Protection

1. On the **Change termination protection** page, check the box labeled **Enable**.
2. Click **Save**.

![6](https://github.com/user-attachments/assets/f7d06a41-c00d-4084-a12f-98b80ad9bfaa)

#### Step 4: Verification (Optional but Recommended)

1. With the instance still selected, look at the **Details** tab at the bottom.
2. Search for the **Termination protection** field; it should now display **Enabled**.
3. Alternatively, attempt to "Terminate" the instance (but do not confirm); AWS should provide a warning that the instance is protected and cannot be terminated until the setting is disabled.

---

### Challenges

* **Feature Confusion:** Users often confuse "Stop Protection" with "Termination Protection." **Termination protection** prevents the instance from being deleted (terminated), while **Stop protection** prevents it from being powered off. Ensure you select the specific protection requested by the team.
* **Permissions:** You must have the `ec2:ModifyInstanceAttribute` permission to change these settings.
* **Instance State:** Unlike changing an instance type, you **do not** need to stop the instance to enable or disable protection; this can be done while the instance is `Running`.

---

### Fun Message

> Safety first! Your `datacenter-ec2` is now wearing its digital seatbelt. Even if someone has a "clumsy thumb" moment in the console, your instance isn't going anywhere. It’s basically the "Are you sure? Are you REALLY sure?" of the cloud world.

👉[Dev.to](https://dev.to/hritikraj8804/aws-108-adding-a-safety-latch-enabling-ec2-stop-protection-4cp7)
