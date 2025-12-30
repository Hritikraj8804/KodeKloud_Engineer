# Instruction

The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.
An IAM user named `iamuser_jim` and a policy named `iampolicy_jim` already exist. Attach the IAM policy `iampolicy_jim` to the IAM user `iamuser_jim`.

# Solution

Follow these steps using the AWS Management Console to link the policy to the user:

#### Step 1: Locate the IAM User

1. Log into the **AWS Management Console**.

![1](https://github.com/user-attachments/assets/c419271e-994f-4524-95bf-0b0f0d9ba339)

2. Navigate to the **IAM Dashboard**.

![2](https://github.com/user-attachments/assets/29eafed2-8689-435d-ad87-13c03a1ac2cf)

3. In the left navigation pane, click on **Users**.

![3](https://github.com/user-attachments/assets/d37a6c9d-6950-45bd-be91-3613ddedb623)

4. From the list of users, click on the name **iamuser_jim**.

![4](https://github.com/user-attachments/assets/a668db9b-b75b-4b38-bc7a-9d911b2bdfc0)


#### Step 2: Add Permissions

1. Inside the user details page, ensure you are on the **Permissions** tab.

![5](https://github.com/user-attachments/assets/e19f698e-055e-4e34-826a-4d83206627b9)

2. Click the **Add permissions** button (it may appear as a dropdown; select **Add permissions**).

#### Step 3: Attach Existing Policy Directly

1. Under the **Permissions options**, select **Attach policies directly**.

![6](https://github.com/user-attachments/assets/8e3dfb0a-0b36-48f2-a3b9-9666ba1db909)

2. In the **Permissions policies** search box, type `iampolicy_jim`.

![7](https://github.com/user-attachments/assets/aaba40b4-b9f7-454a-bece-f31332af6ef5)

3. Check the box next to the policy name **iampolicy_jim** when it appears in the list.

#### Step 4: Review and Attach

1. Click the **Next** button at the bottom of the page.
2. Review the summary to ensure `iamuser_jim` is getting the `iampolicy_jim` policy.
3. Click **Add permissions** to finalize the attachment.

#### Step 5: Verification

1. Return to the **Permissions** tab for `iamuser_jim`.
2. Confirm that **iampolicy_jim** is now listed under the "Permissions policies" section.

![8](https://github.com/user-attachments/assets/108a5c0b-bc18-4eeb-a668-8ba7ef2d6275)


---

### Challenges

* **Policy Types:** Ensure you are attaching the "Customer managed" policy and not a similarly named AWS managed policy if one exists.
* **Propagation Delay:** While IAM changes are usually near-instant, it can sometimes take a few seconds for the new permissions to propagate across all AWS regions.
* **Direct vs. Group Attachment:** While attaching directly to a user works, for larger teams, it is often better to attach the policy to a group and then add the user to that group to keep things organized.

---

### Fun Message

> Success! `iamuser_jim` and `iampolicy_jim` are now officially connected. It's like a digital marriage, but with less cake and more JSON. Jim finally has the power he was promised let's just hope he uses it for good and not for accidentally spinning up 500 expensive instances!

👉[Dev.to](https://dev.to/hritikraj8804/aws-119-making-the-connection-attaching-an-iam-policy-to-a-user-3o3e)
