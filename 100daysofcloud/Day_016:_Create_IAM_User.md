# Instruction

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements:
For this task, create an IAM user named `iamuser_javed`.

# Solution

Follow these steps using the AWS Management Console to create the IAM user:

#### Step 1: Navigate to the IAM Dashboard

1. Log into the **AWS Management Console**.

![1](https://github.com/user-attachments/assets/761d1507-a958-492a-8a32-106c50f7236c)

2. In the search bar at the top, type **IAM** and select the IAM service.

![2](https://github.com/user-attachments/assets/8aa78310-b8d7-46f3-b62e-2272fd015758)

#### Step 2: Access the Users Section

1. In the left navigation pane of the IAM dashboard, click on **Users**.

![3](https://github.com/user-attachments/assets/b6950f4d-8ba2-4771-a8a1-d2b2a2acb38c)

2. Click the **Create user** button located at the top right of the page.

![4](https://github.com/user-attachments/assets/af88594e-87a0-4de3-9e7a-0edd6c86eac2)

#### Step 3: Specify User Details

1. On the **Specify user details** page:
* **User name:** Enter `iamuser_javed`.
* **AWS Management Console access:** Leave this unchecked unless you intend to provide password access immediately. (For this task, we are only creating the user identity).

![5](https://github.com/user-attachments/assets/7a33375c-a954-49a3-a4c1-28c84b2d7862)

2. Click **Next**.

#### Step 4: Set Permissions (Optional)

1. On the **Set permissions** page, you can choose to add the user to a group or attach policies. Since the prompt only specifies creation, you may click **Next** to proceed with a user that has no initial permissions (Least Privilege).

#### Step 5: Review and Create

1. Review the user name to ensure it is exactly `iamuser_javed`.
2. Click **Create user**.

#### Step 6: Verification

1. You should be returned to the **Users** list.
2. Search for `iamuser_javed` to confirm the user appears in the list with the status "Created".

![6](https://github.com/user-attachments/assets/a2b6b3e5-9033-4318-b1b4-c48ef9cc0cd4)

---

### Challenges

* **Case Sensitivity:** IAM usernames are not case-sensitive (e.g., you cannot have `Javed` and `javed`), but for consistency with DevOps documentation, stick to the exact name provided.
* **Console Access vs. Programmatic Access:** Beginners often forget that creating a user doesn't automatically grant them a password or an access key. These must be configured separately under the "Security credentials" tab of the user.
* **Permissions Gap:** A newly created user has **zero permissions** by default. They won't be able to see any AWS resources until you attach a policy or add them to a group.

---

### Fun Message

> Welcome to the team, `iamuser_javed`! You now officially exist in the cloud. Just remember: with great power comes great responsibility... and since we haven't given you any permissions yet, you currently have the power to do absolutely nothing. Enjoy the view!

👉[Dev.to](https://dev.to/hritikraj8804/aws-116-who-goes-there-creating-your-first-iam-user-52gg)
