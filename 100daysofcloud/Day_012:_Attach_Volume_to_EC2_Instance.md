# Instruction

The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.
An instance named `nautilus-ec2` and a volume named `nautilus-volume` already exists in `us-east-1` region. Attach the `nautilus-volume` volume to the `nautilus-ec2` instance, make sure to set the device name to `/dev/sdb` while attaching the volume.

# Solution

Follow these steps to attach the storage volume to your instance:

#### Step 1: Navigate to the Volumes Section

1. Log into the **AWS Management Console**.
2. Ensure your region is set to **us-east-1 (N. Virginia)**.

  ![1](https://github.com/user-attachments/assets/df96b6d0-4f34-4992-aea4-dc3b78b424e9)

3. Go to **EC2 Dashboard** > **Elastic Block Store** > **Volumes**.

![2](https://github.com/user-attachments/assets/7f574253-4eb5-4212-b298-1c501b48eb37)

![3](https://github.com/user-attachments/assets/d804d76c-cf29-41d1-aef1-25f45116887e)

#### Step 2: Identify and Select the Volume

1. Find the volume named **`nautilus-volume`** in the list.

![4](https://github.com/user-attachments/assets/e4461a62-5a91-4508-9d98-5b3359e6f9fa)

2. Check that its **State** is **`Available`** (blue icon). If it is "In-use," it is already attached elsewhere.
3. Note the **Availability Zone** of the volume. Ensure the `nautilus-ec2` instance is in the same zone.

![5](https://github.com/user-attachments/assets/84bb487a-21e3-455f-a049-42428c18b040)

#### Step 3: Attach the Volume

1. Select the checkbox for `nautilus-volume`.
2. Click the **Actions** button and select **Attach volume**.

![6](https://github.com/user-attachments/assets/b7ef4890-3ea5-4588-b356-670bbd41fb12)

#### Step 4: Configure Attachment Settings

1. In the **Attach volume** dialog:
* **Instance:** Click the search box and select the instance named **`nautilus-ec2`**.
* **Device name:** Delete any default value provided and manually type **`/dev/sdb`**.

![7](https://github.com/user-attachments/assets/edfaf38b-990a-44c1-a7a6-473e46226b88)

> **Note:** AWS may display a warning that newer Linux kernels might rename this internally (e.g., to `/dev/xvdb`), but you must enter `/dev/sdb` as per the requirement.


2. Click **Attach volume**.

#### Step 5: Verification

1. Wait a few seconds and refresh the **Volumes** list.
2. Confirm that the **State** for `nautilus-volume` has changed to **`In-use`**.
3. In the **Details** tab for the volume, verify that the attachment information shows it is linked to `nautilus-ec2` as `/dev/sdb`.

![8](https://github.com/user-attachments/assets/0e069243-6775-40d9-8d41-8e5d50c44f31)

---

### Challenges

* **Availability Zone (AZ) Mismatch:** EBS volumes are "locked" to a specific AZ. If your volume is in `us-east-1a` and your instance is in `us-east-1b`, the instance will not appear in the selection list. You would need to create a snapshot and restore it in the correct AZ to fix this.
* **Device Name Mapping:** On modern Nitro-based instances, the OS may see the drive as `/dev/nvme1n1` even if you specify `/dev/sdb` in the console. Always check with `lsblk` inside the instance to see how the kernel mapped it.
* **Volume State:** You cannot attach a volume that is still in the `Creating` or `Deleting` state. It must be `Available`.
* **Permissions:** Ensure your IAM user has the `ec2:AttachVolume` permission.

---

### Fun Message

> Storage attached! Think of it as adding a sidecar to your motorcycle. Your instance now has a little extra room for all its digital luggage. Just remember, `/dev/sdb` might be the address in the console, but your Linux kernel might give it a nickname once it gets inside!

👉[Dev.to](https://dev.to/hritikraj8804/aws-112-plug-and-play-how-to-attach-an-ebs-volume-to-an-ec2-instance-2bo7)
