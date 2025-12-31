# Instruction

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements:
Create an IAM role as below:
1) IAM role name must be `iamrole_mariyam`.
2) Entity type must be AWS Service and use case must be EC2.
3) Attach a policy named `iampolicy_mariyam`.

# Solution

Follow these steps using the AWS Management Console to create the IAM role:

#### Step 1: Navigate to IAM Roles

1. Log into the **AWS Management Console**.

  ![1](https://github.com/user-attachments/assets/49783a7a-bfe2-42bf-b95d-78cdb944a1dd)

2. Search for **IAM** and select it from the services list.

  ![2](https://github.com/user-attachments/assets/52451909-d586-46af-8d47-096ae68c7623)

3. In the left-hand navigation pane, click on **Roles**.

  ![3](https://github.com/user-attachments/assets/59d24b95-0111-4e9e-8dc5-e92cf111ac66)

4. Click the **Create role** button.

  ![4](https://github.com/user-attachments/assets/27a639bb-505d-4ead-ab7d-5e9abf5000c9)


#### Step 2: Select Trusted Entity

1. For **Trusted entity type**, select **AWS service**.
2. Under the **Service or use case** dropdown, select **EC2**.
3. Select the radio button for **EC2** (Allows EC2 instances to call AWS services on your behalf).

  ![5](https://github.com/user-attachments/assets/4d401a30-833c-44eb-8c44-4ec5d227926f)

4. Click **Next**.

#### Step 3: Add Permissions

1. In the **Permissions policies** search box, type `iampolicy_mariyam`.
2. Check the box next to the policy name **iampolicy_mariyam**.
* *Note: If the policy does not exist yet, you may need to create it first using the "Create policy" button.*

  ![6](https://github.com/user-attachments/assets/d64209d9-0178-4021-b8af-3c7531adb79b)

3. Click **Next**.

#### Step 4: Name, Review, and Create

1. For **Role name**, enter `iamrole_mariyam`.
2. **Description (Optional):** You can add a description like "Role for EC2 instances to access resources via iampolicy_mariyam."

  ![7](https://github.com/user-attachments/assets/ded77c8b-12c7-4d86-90cc-35eb4a2fa791)

3. Review the **Step 1: Select trusted entities** section to ensure it lists `ec2.amazonaws.com`.
4. Review the **Step 2: Add permissions** section to ensure `iampolicy_mariyam` is attached.

  ![7 1](https://github.com/user-attachments/assets/cc79b5b4-9009-4894-9728-29e13cf6bc1f)

5. Click **Create role**.

  ![8](https://github.com/user-attachments/assets/1c9a9a6c-9694-4cfc-b4af-be91f1481615)


---

### Challenges

* **Policy Availability:** If `iampolicy_mariyam` was not created beforehand, it will not appear in the search results during Step 3. Ensure the policy name is exact.
* **Trust Relationship:** By selecting the EC2 use case, AWS automatically generates a "Trust Policy" that allows the EC2 service to "Assume" this role. Modifying this JSON manually can be tricky if you aren't familiar with the syntax.
* **Instance Profiles:** When you create an IAM role for EC2 via the console, AWS automatically creates an **Instance Profile** with the same name. This profile is what actually allows you to "attach" the role to a virtual machine.

---

### Fun Message

> High-five! `iamrole_mariyam` is now officially ready for action. Your EC2 instances now have a specialized identity they can wear like a superhero cape to perform tasks securely. Just remember: with great power comes a great need to make sure your JSON syntax is correct!
