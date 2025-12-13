# Instruction

The Nautilus DevOps team is strategizing the migration of a portion of their infrastructure to the AWS cloud. Recognizing the scale of this undertaking, they have opted to approach the migration in incremental steps rather than as a single massive transition. To achieve this, they have segmented large tasks into smaller, more manageable units. This granular approach enables the team to execute the migration in gradual phases, ensuring smoother implementation and minimizing disruption to ongoing operations. By breaking down the migration into smaller tasks, the Nautilus DevOps team can systematically progress through each stage, allowing for better control, risk mitigation, and optimization of resources throughout the migration process.
For this task, create a security group under default VPC with the following requirements:
Name of the security group is nautilus-sg.

The description must be Security group for Nautilus App Servers

Add the inbound rule of type HTTP, with port range of 80. Enter the source CIDR range of 0.0.0.0/0.

Add another inbound rule of type SSH, with port range of 22. Enter the source CIDR range of 0.0.0.0/0.

# Solution

Follow these steps using the AWS Management Console to create the required security group and configure its rules:

#### Step 1: Navigate to the EC2 Dashboard

1.  Log into the **AWS Management Console**.
2.  Ensure you switch your region to **us-east-1 (N. Virginia)**.
3.  Search for and select **EC2** from the Services list to go to the EC2 Dashboard.

   ![ec2](https://github.com/user-attachments/assets/7d71ee4e-e9c7-4184-8764-05ee64208863)

#### Step 2: Access the Security Groups Section

1.  In the navigation pane on the left side of the EC2 Dashboard, under **Network & Security**, click on **Security Groups**.

![security grp](https://github.com/user-attachments/assets/7327b200-15b9-4249-a8e5-24fbcd2ce59a)

#### Step 3: Create the Security Group

1.  Click the **Create security group** button.
2.  On the **Create security group** page:
    * For **Security group name**, enter: `nautilus-sg`.
    * For **Description**, enter: `Security group for Nautilus App Servers`.
    * For **VPC**, ensure the **Default VPC** is selected (this is usually the default option).

![form](https://github.com/user-attachments/assets/7e658334-517e-4fe2-b4bd-15616b6d27ad)


#### Step 4: Configure Inbound Rules

1.  Scroll down to the **Inbound rules** section.
2.  Click the **Add rule** button.
3.  **Configure Rule 1 (HTTP):**
    * For **Type**, select **HTTP**. The **Port range** should automatically populate with `80`.
    * For **Source**, select **Anywhere-IPv4** ($0.0.0.0/0$) from the dropdown or manually enter $0.0.0.0/0$.
4.  Click **Add rule** again.
5.  **Configure Rule 2 (SSH):**
    * For **Type**, select **SSH**. The **Port range** should automatically populate with `22`.
    * For **Source**, select **Anywhere-IPv4** ($0.0.0.0/0$) from the dropdown or manually enter $0.0.0.0/0$.



#### Step 5: Finalize Creation

1.  Click the **Create security group** button at the bottom of the page.
   
![result](https://github.com/user-attachments/assets/7ffec882-87df-4051-bd91-2a6b957d84ce)

#### Step 6: Verification

1.  You should now see the newly created security group, `nautilus-sg`, listed on the **Security Groups** page, and its inbound rules will reflect the HTTP (80) and SSH (22) ports open to the internet.

---

### Challenges

Here are some potential issues or common mistakes to watch out for:

* **Incorrect Inbound Rule Ports:** Using port 8080 instead of the required port **80** for HTTP, or an incorrect port for SSH.
* **Restricted Source CIDR:** Accidentally setting the source to **My IP** or a specific CIDR range, instead of the required **$0.0.0.0/0$** (Anywhere).
* **Outbound Rules:** Trying to modify or add specific outbound rules. This task only requires inbound rules; the default outbound rule (Allow all traffic) is usually sufficient.
* **VPC Selection:** Creating the Security Group in a non-default VPC when the requirement specified the **default VPC**.

---

### Funny Message

> Another successful security measure! Now your app servers can chat with the whole world on port 80 and let their admin friends in on port 22. Just be sure to always lock your doors (i.e., use less permissive sources in production)!
