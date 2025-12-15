# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition.
For this task, create one subnet named nautilus-subnet under default VPC.

---
# Solution

Follow these steps using the AWS Management Console to create the required subnet:

#### Step 1: Navigate to the VPC Dashboard

1.  Log into the **AWS Management Console** using the provided credentials.
2.  Ensure your region is set to **us-east-1 (N. Virginia)**.
3.  Search for and select **VPC** from the Services list to go to the VPC Dashboard.

   ![1](https://github.com/user-attachments/assets/760acc36-cf18-44bf-91b9-eef45f6e0336)

#### Step 2: Initiate Subnet Creation

1.  In the navigation pane on the left, under **Virtual private cloud**, click on **Subnets**.   

![2](https://github.com/user-attachments/assets/fddb11c3-3838-4fdf-ae00-7c98b6405f53)

2.  Click the **Create subnet** button.

![3](https://github.com/user-attachments/assets/47b15eaf-10eb-4268-9dae-66f5283e0451)

#### Step 3: Configure Subnet Details

1.  On the **Create subnet** page:
    * **VPC ID:** From the dropdown, select your **Default VPC**. (It will typically have a name like `vpc-xxxxxxxxxxxxxxxx`).
2.  In the **Subnet settings** section:
    * **Subnet name:** Enter the required name: `nautilus-subnet`.
    * **Availability Zone:** Select any Availability Zone in the `us-east-1` region, such as **us-east-1a**.
    * **IPv4 CIDR block:** Enter the recommended non-conflicting block: **$172.31.64.0/24$**.



#### Step 4: Finalize Creation

1.  Click the **Create subnet** button at the bottom of the page.

#### Step 5: Verification

1.  Navigate back to the **Subnets** list.
2.  Confirm that a subnet named `nautilus-subnet` has been successfully created with the CIDR block $172.31.64.0/24$ and is associated with the Default VPC.

![4](https://github.com/user-attachments/assets/9ee0635e-45a9-4537-97ff-cf93a5872411)

---

### Challenges

Here are some potential issues or common mistakes to watch out for:

* **CIDR Overlap Error:** Attempting to use a CIDR block that overlaps with an existing subnet (as experienced previously). The use of **$172.31.64.0/24$** should prevent this.
* **Incorrect VPC Selection:** Selecting a custom VPC instead of the required **Default VPC**.
* **Missing Name Tag:** Forgetting to add the **Name tag** with the value `nautilus-subnet`, which is crucial for identification and organization.
* **Incorrect Region/AZ:** Creating the subnet in the wrong region or accidentally selecting an Availability Zone that is not within the specified region (though AWS usually restricts this).

---

### Fun Message

> Congratulations, you've successfully carved out a little corner of the cloud! Now, your EC2 instances have a nice little neighborhood to call home. Just make sure they don't host any block parties—we're trying to migrate, not cause a traffic jam!

👉[Dev.to](https://dev.to/hritikraj8804/aws-103-mapping-your-cloud-neighborhood-creating-your-first-subnet-4h47)
> 
