# Instruction

When establishing infrastructure on the AWS cloud, Identity and Access Management (IAM) is among the first and most critical services to configure. IAM facilitates the creation and management of user accounts, groups, roles, policies, and other access controls. The Nautilus DevOps team is currently in the process of configuring these resources and has outlined the following requirements.
Create an IAM policy named `iampolicy_mark` in `us-east-1` region, it must allow read-only access to the EC2 console, i.e this policy must allow users to view all instances, AMIs, and snapshots in the Amazon EC2 console.

# Solution

Follow these steps using the AWS Management Console to create the custom IAM policy:

#### Step 1: Navigate to IAM Policies

1. Log into the **AWS Management Console**.

![1](https://github.com/user-attachments/assets/6c13eee6-1e03-47d9-b23c-01f2755f52dc)

2. Search for **IAM** and select it.

![2](https://github.com/user-attachments/assets/f602c2ae-df17-4619-976b-93bb163d9a7c)

3. In the left navigation pane, click on **Policies**.

![3](https://github.com/user-attachments/assets/d28ac909-ac3a-4b49-b50b-0e9a370a3c29)

4. Click the **Create policy** button.

![4](https://github.com/user-attachments/assets/cbfc8101-2bb4-4447-ae08-591c38b49619)


#### Step 2: Define Permissions (JSON Method)

While the Visual Editor is available, using the JSON editor ensures all specific requirements (Instances, AMIs, Snapshots) are captured accurately.

1. Select the **JSON** tab.
2. Paste the following policy document into the editor:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ec2:DescribeInstances",
                "ec2:DescribeImages",
                "ec2:DescribeSnapshots",
                "ec2:DescribeTags"
            ],
            "Resource": "*"
        }
    ]
}

```

![5](https://github.com/user-attachments/assets/75b8a0d1-2cd0-4de9-8522-feec54b5f027)

3. Click **Next**.

#### Step 3: Name and Review

1. **Policy name:** Enter `iampolicy_mark`.
2. **Description (Optional):** "Read-only access to EC2 instances, AMIs, and snapshots."
3. Ensure that the Summary shows **EC2** with "Limited: List, Read" permissions.

![6](https://github.com/user-attachments/assets/2d9b0812-f5c9-4f75-a780-a105eb724fba)


#### Step 4: Create Policy

1. Click **Create policy** at the bottom of the page.

#### Step 5: Verification

1. Search for `iampolicy_mark` in the search filter on the Policies page.
2. Click on the policy name to verify that the permissions match the requirements.

![7](https://github.com/user-attachments/assets/f57d7484-652d-4da8-b576-9e3f148e82c2)


---

### Challenges

* **Action Specificity:** There are hundreds of EC2 "Describe" actions. While `AmazonEC2ReadOnlyAccess` (an AWS Managed Policy) covers these, creating a *custom* policy requires selecting the specific `Describe` actions manually or via JSON.
* **JSON Syntax:** Missing a comma or a bracket in the JSON editor will result in an error. Always use the "Validate policy" feature if available.
* **Global vs Regional:** IAM policies are **Global** entities, but their effects apply to resources across all regions unless specifically restricted in the JSON `Condition` block.

---

### Fun Message

> Look at you, crafting custom security rules like a digital blacksmith! `iampolicy_mark` is now live. It’s the perfect "look but don't touch" policy. It’s basically giving someone the keys to the museum, but locking all the display cases in bulletproof glass. Happy (and safe) viewing!

👉[Dev.to](https://dev.to/hritikraj8804/aws-118-setting-the-rules-creating-a-custom-iam-policy-5694)
