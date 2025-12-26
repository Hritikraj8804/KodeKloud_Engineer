# Instruction

The Nautilus DevOps team has some volumes in different regions in their AWS account. They are going to setup some automated backups so that all important data can be backed up on regular basis. For now they shared some requirements to take a snapshot of one of the volumes they have.
Create a snapshot of an existing volume named `datacenter-vol` in `us-east-1` region.
1) The name of the snapshot must be `datacenter-vol-ss`.
2) The description must be `datacenter` Snapshot.
3) Make sure the snapshot status is completed before submitting the task.

# Solution

Follow these steps to create the snapshot and verify its completion:

#### Step 1: Locate the Volume

1. Log into the **AWS Management Console**.
2. Ensure your region is set to **us-east-1 (N. Virginia)**.

![1](https://github.com/user-attachments/assets/f36d6c03-6102-4251-8028-7a704cb0e2ed)

3. Navigate to **EC2 Dashboard** > **Elastic Block Store** > **Volumes**.

![2](https://github.com/user-attachments/assets/54be0557-9b5b-4ec2-a1bb-fe30f94c8e40)

![3](https://github.com/user-attachments/assets/3a835dcd-d48d-42f4-a2b6-4b330d34b1e8)

4. Find the volume named `datacenter-vol` in the list.

![4](https://github.com/user-attachments/assets/a93fd2be-2ad7-4888-babe-f44fed04adb5)

#### Step 2: Initiate Snapshot Creation

1. Select the checkbox for `datacenter-vol`.
2. Click the **Actions** button and select **Create snapshot**.

![5](https://github.com/user-attachments/assets/26a0d331-6437-4d07-b310-48a10cdc07dc)

#### Step 3: Configure Snapshot Details

1. On the **Create snapshot** page:
* **Description:** Enter `datacenter Snapshot`.

![6](https://github.com/user-attachments/assets/40d0308b-6c15-4a18-935b-cd2191e7c93e)

2. In the **Tags** section:
* Click **Add tag**.
* **Key:** `Name`
* **Value:** `datacenter-vol-ss`


3. Click the **Create snapshot** button.

#### Step 4: Monitor and Verify Status

1. In the left navigation pane, under **Elastic Block Store**, click on **Snapshots**.

![7](https://github.com/user-attachments/assets/c1b0800e-20d9-42f5-ae69-0f164271a9f3)

2. Locate your new snapshot `datacenter-vol-ss`.
3. Monitor the **Status** column. It will initially show as `pending` with a progress percentage.
4. Refresh the page until the status changes to **`completed`**.

![8](https://github.com/user-attachments/assets/d74b636f-b808-4b8b-85e9-ca8bcee8b4a3)

---

### Challenges

* **Size vs. Time:** Larger volumes (or volumes with many changes since the last snapshot) will take longer to complete. Don't be alarmed if it stays at "Pending" for a few minutes.
* **Performance Impact:** While snapshots are being taken, there can be a very slight increase in I/O latency for the instance attached to the volume.
* **Snapshot Costs:** Remember that snapshots are billed based on the amount of data stored in Amazon S3. Deleting old or unnecessary snapshots is a key part of cost optimization.
* **Region Lock:** Like the volume itself, the snapshot resides in the region where it was created. If you need it elsewhere, you must use the "Copy Snapshot" action.

---

### Fun Message

> Smile! You've just taken a high-definition photo of your data. It's now safely tucked away in S3, probably hanging out with other snapshots and telling stories about the "good old days" before the migration.

👉[Dev.to](https://dev.to/hritikraj8804/aws-115-your-datas-safety-net-creating-an-ebs-snapshot-4ikc)
