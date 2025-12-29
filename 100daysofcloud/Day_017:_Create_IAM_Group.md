# Instruction

The Nautilus DevOps team has been creating a couple of services on AWS cloud. They have been breaking down the migration into smaller tasks, allowing for better control, risk mitigation, and optimization of resources throughout the migration process. Recently they came up with requirements mentioned below.
Create an IAM group named `iamgroup_kirsty`.

# Solution

Follow these steps using the AWS Management Console to create the IAM group:

#### Step 1: Navigate to IAM User Groups

1. Log into the **AWS Management Console**.

![1](https://github.com/user-attachments/assets/bf411dc9-c9ef-42bb-ab5a-d374eb2a0701)

2. Navigate to the **IAM Dashboard**.

![2](https://github.com/user-attachments/assets/029ee9f6-4538-433e-ab33-3044e2243b27)

3. In the left navigation pane, click on **User groups**.

![3](https://github.com/user-attachments/assets/ff999dd3-35f9-49c7-8d7c-a91df3b5cf22)


#### Step 2: Create Group

1. Click the **Create group** button.

![4](https://github.com/user-attachments/assets/2cf1dcc5-6aea-4e34-a002-131a052fc906)

2. On the **Create user group** page, locate the **User group name** field.
3. Enter the name `iamgroup_kirsty`.

![5](https://github.com/user-attachments/assets/f4ab9190-5802-485f-a18b-4898c768ee77)


#### Step 3: Review and Finalize

1. **Add users to the group:** You may skip this step for now (leave all users unchecked).
2. **Attach permissions policies:** You may skip this step for now (do not select any policies).
3. Scroll to the bottom and click **Create group**.

#### Step 4: Verification

1. In the **User groups** list, search for `iamgroup_kirsty`.
2. Confirm that the group appears and has a "Users" count of **0** and "Attached policies" count of **0**.

![6](https://github.com/user-attachments/assets/2736ced1-b138-41f1-bde2-4e756e3c7bea)

---

### Challenges

* **Naming Conventions:** Ensure there are no trailing spaces in the name `iamgroup_kirsty`, as AWS treats spaces as characters which might conflict with automation scripts.
* **Inheritance:** Remember that any user added to this group later will automatically inherit all permissions attached to the group. If the group has no permissions, the users will remain "Permission-less" despite being in a group.
* **Group Limits:** While not a concern for a single group, AWS has a default limit of 300 groups per account.

---

### Fun Message

> And just like that, `iamgroup_kirsty` is born! It’s currently an empty vessel no members, no permissions, just a lonely name in a vast cloud. It’s basically the digital equivalent of an empty VIP lounge. Now, who are we going to let past the velvet rope?

[Dev.to](https://dev.to/hritikraj8804/aws-117-managing-the-crowd-creating-your-first-iam-group-2pf4)
