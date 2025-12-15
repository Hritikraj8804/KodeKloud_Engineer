# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.
For this task, allocate an Elastic IP address, name it as datacenter-eip.

# Solution

Follow these steps using the AWS Management Console to allocate the required Elastic IP address:

#### Step 1: Navigate to the EC2 Dashboard

1.  Log into the **AWS Management Console**.
2.  Ensure your region is set to **us-east-1 (N. Virginia)**.
3.  Search for and select **EC2** from the Services list to go to the EC2 Dashboard.

![1](https://github.com/user-attachments/assets/a71df7a4-07b5-465c-8314-91ebecd468c3)

#### Step 2: Access the Elastic IPs Section

1.  In the navigation pane on the left side, under **Network & Security**, click on **Elastic IPs**.

![2](https://github.com/user-attachments/assets/6542618f-8762-4cd1-af2e-a5fbb4ed6234)



#### Step 3: Allocate the Elastic IP

1.  Click the **Allocate Elastic IP address** button.

![3](https://github.com/user-attachments/assets/fa275765-d415-4c9b-97e5-5e6c5889cd6e)

2.  On the **Allocate Elastic IP address** page:
    * **Network border group:** Ensure this is set to the default for your region (e.g., `us-east-1`).
    * **Public IPv4 address pool:** Select **Amazon's pool of IPv4 addresses**. (This is the default and sufficient for the task).
    ![4](https://github.com/user-attachments/assets/23ed3680-2eae-48eb-9174-d06fa33867d3)


#### Step 4: Add the Required Name Tag

1.  Scroll down to the **Tags** section.
2.  Click the **Add new tag** button.
    * For **Key**, enter: `Name` (case-sensitive).
    * For **Value**, enter the required name: `datacenter-eip`.

![5](https://github.com/user-attachments/assets/f0f3a71f-a9c2-4730-9e5c-6a7c59626d1d)


#### Step 5: Finalize Allocation

1.  Click the **Allocate** button at the bottom of the page.

   ![6](https://github.com/user-attachments/assets/d29141ff-a2e1-4a52-a56a-8f8725b0a1d9)


#### Step 6: Verification

1.  You will be redirected to the **Elastic IPs** list.
2.  Confirm that a new EIP has been allocated (it will show a public IPv4 address) and has the tag **Name: `datacenter-eip`**.

---

### Challenges

Here are some potential issues or common mistakes to watch out for:

* **EIP Limit:** While unlikely for a single allocation, remember that AWS accounts have a default limit on the number of EIPs you can allocate (usually 5). Exceeding this limit requires a service quota increase.
* **Region Mismatch:** Allocating the EIP in the wrong region. EIPs are regional resources and cannot be moved between regions.
* **Forgetting the Tag:** Skipping the step to add the required **Name** tag (`datacenter-eip`), which makes the resource harder to identify.
* **Unused EIP Charge:** While not immediately a challenge, be aware that AWS charges for Elastic IPs that are **allocated but not associated** with a running EC2 instance or a NAT Gateway. *Best practice is to release the EIP when it's no longer needed.*

---

### Fun Message

> You've successfully claimed a permanent piece of internet real estate! Treat this IP address well; it's static, reliable, and unlike most teenagers, it won't change its address without notice.

👉[Dev.to](https://dev.to/hritikraj8804/aws-104-never-change-your-address-allocating-an-elastic-ip-eip-1lcg)
