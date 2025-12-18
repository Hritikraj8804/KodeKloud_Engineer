# Instruction

During the migration process, the Nautilus DevOps team created several EC2 instances in different regions. They are currently in the process of identifying the correct resources and utilization and are making continuous changes to ensure optimal resource utilization. Recently, they discovered that one of the EC2 instances was underutilized, prompting them to decide to change the instance type. Please make sure the Status check is completed (if its still in Initializing state) before making any changes to the instance.
1) Change the instance type from t2.micro to t2.nano for nautilus-ec2 instance.
2) Make sure the ec2 instance nautilus-ec2 is in running state after the change.

# Solution
Follow these steps to modify the instance type of your EC2 instance:

#### Step 1: Verify Instance Health

![1](https://github.com/user-attachments/assets/6679b82c-c125-4850-89d1-715ed9511732)

1. Navigate to the **EC2 Dashboard** and click on **Instances**.

![2](https://github.com/user-attachments/assets/edc8d1b4-5bab-428a-b178-fabd019bf36c)

![3](https://github.com/user-attachments/assets/a204b375-5e63-454f-9e50-f5cf4e14288e)

2. Locate `nautilus-ec2`. Look at the **Status check** column.

![4](https://github.com/user-attachments/assets/990849f4-2bea-4aa0-9a1b-70823ad08ba4)

3. Wait until it shows **"2/2 checks passed"**. Do not proceed while it says "Initializing".

![5](https://github.com/user-attachments/assets/d6251d03-cd5c-42f1-9c08-a433fd0e71e6)

#### Step 2: Stop the Instance

1. Select the checkbox next to `nautilus-ec2`.
2. Click the **Instance state** button at the top and select **Stop instance**.

![6](https://github.com/user-attachments/assets/3ab894f2-92aa-4266-8687-c70a606f94fe)

3. Confirm the action by clicking **Stop** in the pop-up.

![7](https://github.com/user-attachments/assets/b8ed66d4-3049-42fc-b29a-880db2ca7880)

4. Wait for the **Instance state** to change from `Stopping` to **`Stopped`**.

![8](https://github.com/user-attachments/assets/9a375358-367f-4538-8c9f-f7631be19cab)

#### Step 3: Change the Instance Type

1. With the instance still selected (and in the `Stopped` state), click the **Actions** button.
2. Navigate to **Instance settings** and select **Change instance type**.

![9](https://github.com/user-attachments/assets/ebde1cfe-0899-4393-a7b0-37001facae34)

3. In the **Change instance type** dialog:
* Select **t2.nano** from the **Instance type** dropdown menu.

![10](https://github.com/user-attachments/assets/0ffdf4e1-3e90-4890-b3dd-0212dd1e4d18)

4. Click the **Apply** button.

#### Step 4: Restart the Instance

1. With `nautilus-ec2` selected, click the **Instance state** button.
2. Select **Start instance**.

![11](https://github.com/user-attachments/assets/ffc97fe2-76af-4352-8cdc-8eba9e742b61)

3. Wait for the **Instance state** to show **`Running`**.

![12](https://github.com/user-attachments/assets/7827c143-ed34-4bde-9850-7e796a752f04)

#### Step 5: Verification

1. Check the **Instance type** column for `nautilus-ec2`. It should now display **`t2.nano`**.
2. Ensure the **Instance state** is green and says **`Running`**.

---

### Challenges

* **Forgetting to Stop:** You cannot change the instance type while the instance is running. The "Change instance type" option will be greyed out or hidden.
* **Patience with Status Checks:** Modifying an instance while it is still initializing can lead to state transition errors. Always wait for the "2/2 checks passed" mark.
* **Data in Instance Store:** While not applicable to `t2` instances (which use EBS), remember that stopping an instance that uses "Instance Store" volumes will result in data loss on those specific ephemeral volumes.
* **Public IP Change:** If you are not using an Elastic IP, stopping and starting the instance will cause the **Public IPv4 address** to change.

---

### Fun Message

> Downsizing complete! Your `nautilus-ec2` is now a `t2.nano`. Think of it as your server going on a minimalist diet—less memory, less CPU, but much better for the company wallet. Just don't ask it to run anything too heavy, or it might start sweating!

👉[Dev.to](https://dev.to/hritikraj8804/aws-107-save-money-by-rightsizing-how-to-change-an-ec2-instance-type-349o)
