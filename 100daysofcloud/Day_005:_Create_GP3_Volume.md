# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.

Create a volume with the following requirements:
Name of the volume should be `nautilus-volume`.

Volume type must be `gp3`.

Volume size must be `2 GiB`.

# Solution

Follow these steps using the AWS Management Console to create the required EBS volume:

#### Step 1: Navigate to the EC2 Dashboard

1.  Log into the **AWS Management Console**.
2.  Ensure your region is set to **us-east-1 (N. Virginia)**.
3.  Search for and select **EC2** from the Services list to go to the EC2 Dashboard.

![1](https://github.com/user-attachments/assets/82de84f6-2e17-457c-b3fb-af699e54b863)

#### Step 2: Access the Volumes Section

1.  In the navigation pane on the left side, under **Elastic Block Store**, click on **Volumes**.

![2](https://github.com/user-attachments/assets/f0f472ac-2065-40ac-85c9-0d65ec4e462a)

#### Step 3: Initiate Volume Creation

1.  Click the **Create volume** button.

![3](https://github.com/user-attachments/assets/d6112a4e-fd35-43c9-b8b5-e2fd8b227762)

#### Step 4: Configure Volume Settings

1.  On the **Create volume** page, configure the following settings:
    * **Volume type:** Select **General Purpose SSD (gp3)**.
    * **Size (GiB):** Enter the required size: `2`. (The default IOPS and throughput values for gp3 will automatically populate).
    * **Availability Zone:** Select the same Availability Zone where your instance or subnet resides (e.g., **us-east-1a**). **Crucially, the volume must be in the same AZ as the instance it will be attached to.**

![4](https://github.com/user-attachments/assets/bfdea37f-debd-40a8-8eec-c218490483bb)

#### Step 5: Add the Required Name Tag

1.  Scroll down to the **Tags** section.
2.  Click the **Add new tag** button.
    * For **Key**, enter: `Name` (case-sensitive).
    * For **Value**, enter the required name: `nautilus-volume`.

![5](https://github.com/user-attachments/assets/69b0b98f-f7c4-4042-921f-6ff6febe044e)

#### Step 6: Finalize Creation

1.  Click the **Create volume** button at the bottom of the page.

#### Step 7: Verification

1.  You will be redirected back to the **Volumes** list.
2.  Confirm that a new volume is listed with the status `creating` (which should quickly change to `available`), size **2 GiB**, type **gp3**, and the name tag **`nautilus-volume`**.

![6](https://github.com/user-attachments/assets/fa7e6e25-ba57-4a92-9023-fd8710fb54bb)

---

### Challenges

Here are some potential issues or common mistakes to watch out for:

* **Incorrect Volume Type:** Accidentally choosing `gp2` or `io1` instead of the required **`gp3`**.
* **Size Error:** Entering a size other than the required **2 GiB**.
* **Availability Zone Mismatch:** Creating the volume in an Availability Zone (AZ) different from the instance it is intended for. EBS volumes are **AZ-specific** and cannot be attached across AZs.
* **Forgetting the Tag:** Not adding the required **Name** tag, making the resource management difficult.

---

### Fun Message

> Your shiny new digital brick is ready! It's small, it's fast (thanks to gp3!), and it's perfectly prepared to hold all the important data (or just that one really big log file). Just don't drop it!

👉[Dev.to](https://dev.to/hritikraj8804/aws-105-persistent-storage-power-up-creating-an-ebs-volume-2n0g)
