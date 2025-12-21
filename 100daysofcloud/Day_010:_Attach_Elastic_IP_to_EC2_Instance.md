# Instruction

The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.
There is an instance named `datacenter-ec2` and an elastic-ip named `datacenter-ec2-eip` in `us-east-1` region. Attach the `datacenter-ec2-eip` elastic-ip to the `datacenter-ec2` instance


# Solution

Follow these steps to allocate the IP and link it to your instance:

#### Step 1: Allocate the Elastic IP

1. Log into the **AWS Management Console**.

  > follow this steps to [Allocate Elastic IP](./Day_004:_Allocate_Elastic_IP.md) if Elastic IP is not already Aloocated to your account.

#### Step 2: Associate the EIP with the Instance

1. Select the newly created `datacenter-ec2-eip` from the list.

![4](https://github.com/user-attachments/assets/473cf4d6-0d68-4509-b06e-176926aafb11)

2. Click the **Actions** button and select **Associate Elastic IP address**.

![5](https://github.com/user-attachments/assets/a3403d5d-e09c-47a9-811a-2bec923443b8)

3. On the association page:
* **Resource type:** Select **Instance**.
* **Instance:** Click the search box and select the instance named `datacenter-ec2`.
* **Private IP address:** Select the private IP of the instance that appears in the dropdown.

![6](https://github.com/user-attachments/assets/d3e0401a-e489-42a6-9d2e-ef12ffff6cc2)

4. Click **Associate**.

#### Step 3: Verification

1. Navigate to the **EC2 Instances** page.
2. Select `datacenter-ec2`.
3. In the **Details** tab, verify that the **Elastic IP addresses** field shows the new IP address.

---

### Challenges

* **Disassociation of Previous IP:** When you associate an EIP with an instance that already has a standard public IPv4 address, the old public IP is released back into Amazon's pool.
* **Region Mismatch:** You cannot associate an EIP created in `us-east-1` with an instance sitting in `us-west-2`. Ensure both are in **N. Virginia**.
* **Unassociated EIP Costs:** AWS charges a small hourly fee for Elastic IPs that are allocated but **not** associated with a running instance. Always associate them immediately after allocation to avoid unexpected charges.
* **Instance State:** The instance should ideally be in a `running` or `stopped` state to perform the association.

![7](https://github.com/user-attachments/assets/94d29673-7b98-427b-a2f7-de10d5ccec85)


---

### Fun Message

> Success! Your instance now has a permanent home on the internet. It’s like giving your server a VIP backstage pass it doesn't matter how many times it leaves the stage (reboots), it always gets back in through the same door!

👉[Dev.to](https://dev.to/hritikraj8804/aws-110-making-it-permanent-how-to-associate-an-elastic-ip-with-an-ec2-instance-1ioi)
