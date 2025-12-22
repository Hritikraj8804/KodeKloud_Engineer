# Instruction

The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.
An instance named `datacenter-ec2` and an elastic network interface named `datacenter-eni` already exists in `us-east-1` region.
Attach the `datacenter-eni` network interface to the `datacenter-ec2` instance.
Make sure status is attached before submitting the task.
Please make sure instance initialisation has been completed before submitting this task.

# Solution

Follow these steps to attach the secondary network interface to your instance:

#### Step 1: Verify Instance Readiness

1. Log into the **AWS Management Console** and go to the **EC2 Dashboard**.
2. Ensure your region is **us-east-1**.

  ![1](https://github.com/user-attachments/assets/dc2bfd75-0880-4bcd-8724-dd89094e7c76)

  ![2](https://github.com/user-attachments/assets/bc6c60fa-fdd7-4bf1-af16-c03dd62c1186)

3. Click on **Instances** and find `datacenter-ec2`.

  ![4](https://github.com/user-attachments/assets/f9d0ccc9-e513-429d-8741-d53cc97cc007)

4. Verify that the **Status check** column shows **"2/2 checks passed"**.

#### Step 2: Attach the Network Interface

1. In the left navigation pane, under **Network & Security**, click on **Network Interfaces**.

  ![5](https://github.com/user-attachments/assets/3581f5db-0917-4240-8d30-fe0857461408)

2. Find the ENI named `datacenter-eni`. Verify it is in the **"available"** state.

  ![6](https://github.com/user-attachments/assets/e0be2917-e2d7-41c0-9562-f8cf879cf4f5)

3. Select the `datacenter-eni`, click the **Actions** button, and select **Attach**.

  ![7](https://github.com/user-attachments/assets/38559f90-33e4-4909-9861-fbc3b371ddfc)

4. In the **Attach network interface** dialog:
* **Instance ID:** Search for and select `datacenter-ec2`.

  ![8](https://github.com/user-attachments/assets/0dcd173d-25fe-4997-a9f5-07867f1c3e8d)

5. Click **Attach**.

#### Step 3: Verification

1. While still in the **Network Interfaces** screen, refresh the list.
2. Confirm that the status of `datacenter-eni` has changed from "available" to **"in-use"**.
3. Navigate back to **Instances**, select `datacenter-ec2`, and check the **Networking** tab. You should now see two network interfaces listed (eth0 and eth1).

  ![9](https://github.com/user-attachments/assets/5da0fbe2-eedd-4711-bfde-d5df5bb296f6)

---

### Challenges

* **Availability Zone Mismatch:** This is the most common issue. An ENI can only be attached to an instance if they are both in the same Availability Zone (e.g., both must be in `us-east-1a`).
* **Instance State:** While ENIs can typically be attached to running instances ("hot-plugging"), some older instance types might require the instance to be stopped.
* **Max Interface Limit:** Every EC2 instance type has a limit on how many ENIs can be attached. A `t2.micro`, for example, only supports 2 ENIs.
* **Subnet Constraints:** The ENI must belong to a subnet that is compatible with the VPC of the instance.

---

### Fun Message

> Two network cards are better than one! Your instance now has twice the "ears" to listen to traffic. It's officially a multi-tasker just don't let the packets get confused about which door to enter!

👉[Dev.to](https://dev.to/hritikraj8804/aws-111-doubling-your-connectivity-how-to-attach-a-secondary-eni-to-ec2-4k2l)
