# Instruction

As part of the migration, there were some components created under the AWS account. The Nautilus DevOps team created one EC2 instance where they forgot to enable the termination protection which is needed for this instance.
An instance named `xfusion-ec2` already exists in `us-east-1 region`. Enable termination protection for the same.

# Solution

Follow these steps to protect the `xfusion-ec2` instance from accidental termination:

#### Step 1: Locate the Instance

1. Log into the **AWS Management Console**.
2. Set your region to **us-east-1 (N. Virginia)**.

![1](https://github.com/user-attachments/assets/ee5a3ba4-38ed-436e-a117-7c3ccfe7c741)

![2](https://github.com/user-attachments/assets/03b0955b-f861-474e-9b4d-63ad76a8e2c2)

3. Navigate to the **EC2 Dashboard** and click on **Instances**.

![3](https://github.com/user-attachments/assets/89492dfb-dbfa-4399-809f-884c25397822)

4. In the search bar, type `xfusion-ec2` to find the target instance.

![4](https://github.com/user-attachments/assets/419dca98-417b-43ab-ad3b-d2fd25fd789b)


#### Step 2: Modify Termination Protection

1. Select the checkbox next to the `xfusion-ec2` instance.

![4](https://github.com/user-attachments/assets/21c14c2c-99d9-4475-a6e9-843e3684e4b3)

2. Click on the **Actions** dropdown menu.
3. Navigate to **Instance settings** and select **Change termination protection**.

![5](https://github.com/user-attachments/assets/847234c5-aa3b-43d8-9f79-37e198047406)

#### Step 3: Enable and Save

1. In the **Change termination protection** dialog box, check the **Enable** box.
2. Click **Save**.

![6](https://github.com/user-attachments/assets/58c53f55-c024-4758-8c21-38e558556442)

#### Step 4: Verification

1. Select the instance and look at the **Details** tab in the bottom pane.
2. Scroll down to the **Host and placement** or **Instance details** section to find the **Termination protection** field.
3. Confirm that it now says **Enabled**.

![6 5](https://github.com/user-attachments/assets/a3288874-ff82-4b4f-a87a-9b51780b16eb)

![7](https://github.com/user-attachments/assets/63bf852f-bf7c-4bf6-8614-477602cd3619)

---

### Challenges

* **Accidental Termination during Setup:** If you accidentally click "Terminate" instead of selecting the instance for modification, the instance will be deleted permanently. Always double-check your selection.
* **Confusion with Stop Protection:** Ensure you are selecting **Termination protection**. "Stop protection" prevents the instance from being powered down, but "Termination protection" is what prevents the instance from being deleted from the account.
* **IAM Permissions:** If the "Change termination protection" option is greyed out, your IAM user may lack the `ec2:ModifyInstanceAttribute` permission.

---

### Fun Message

> There we go! `xfusion-ec2` is now officially "un-delete-able." It's like putting your favorite mug in a glass case—everyone can see it, but nobody can throw it away by mistake. Stay safe out there!

Would you like me to show you how to create a CloudWatch alarm to notify you if someone *tries* to disable this protection?


👉[Dev.to](https://dev.to/hritikraj8804/aws-109-the-ultimate-safety-net-enabling-ec2-termination-protection-3op7)
