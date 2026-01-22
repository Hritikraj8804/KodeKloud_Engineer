# Instruction

The Nautilus DevOps team is developing a simple 'To-Do' application using DynamoDB to store and manage tasks efficiently. The team needs to create a DynamoDB table to hold tasks, each identified by a unique task ID. Each task will have a description and a status, which indicates the progress of the task (e.g., 'completed' or 'in-progress').

Your task is to:

Create a DynamoDB table named `xfusion-tasks` with a primary key called taskId (string).

Insert the following tasks into the table:Task 1: `taskId`: '`1`', `description`: '`Learn DynamoDB`', `status`: '`completed`'

Task 2: `taskId`: '`2`', `description`: '`Build To-Do App`', `status`: '`in-progress`'

Verify that Task 1 has a status of '`completed`' and Task 2 has a status of '`in-progress`'.

Ensure the DynamoDB table is created successfully and that both tasks are inserted correctly with the appropriate statuses.

# Solution

### **Step 1: Create the Table**

1. Log in to the **AWS Management Console** and search for **DynamoDB**.
2. On the dashboard, click the **Create table** button.
3. **Table details:**
* **Table name:** `xfusion-tasks`
* **Partition key:** `taskId` (Set the type to **String**)
4. **Table settings:** Keep **Default settings** selected (this automatically sets up a Partition Key and handles capacity).
5. Scroll to the bottom and click **Create table**.
* *Wait for the status to change from **Creating** to **Active** (usually takes less than 30 seconds).*

![1](https://github.com/user-attachments/assets/a89412af-b341-4527-ba20-91f393fb2301)

![1 1](https://github.com/user-attachments/assets/da752e5f-967f-44b1-b4c4-b306d0c13649)

![1 2](https://github.com/user-attachments/assets/5fd5b8a5-ac9c-47e8-bc66-cf43eb639caf)

---

### **Step 2: Insert the Tasks**

Once the table is active, you can add your data manually:

1. Click on the table name **`xfusion-tasks`** from the list.
2. Click the **Explore table items** button in the top right.
3. Click **Create item**.

  ![2](https://github.com/user-attachments/assets/1d7a0689-745f-46c9-aad5-48e4048d6f37)

4. **For Task 1:**
* **taskId (String):** `1`
* Click **Add new attribute** > **String**.
* **Attribute name:** `description` | **Value:** `Learn DynamoDB`
* Click **Add new attribute** > **String**.
* **Attribute name:** `status` | **Value:** `completed`
    
    ![2 1](https://github.com/user-attachments/assets/4a87ddbe-ee8c-41b7-a552-68fc3b56a54a)

* Click **Create item**.
5. **For Task 2:**
* Repeat the steps above:
* **taskId:** `2`
* **description:** `Build To-Do App`
* **status:** `in-progress`

  ![2 2](https://github.com/user-attachments/assets/457583ad-95cf-4a26-9cc9-146da409c3cd)

* Click **Create item**.
---

### **Step 3: Verification**

The **Explore items** view will now show a table with your two tasks. You can clearly see the `taskId`, `description`, and `status` columns.
  
  ![3](https://github.com/user-attachments/assets/99b7f759-07a3-4d00-8065-ec95af1900c3)

  ![3 1](https://github.com/user-attachments/assets/4c856e5f-be3e-4504-98f5-b393310ddb04)

---

👉[Dev.to](https://dev.to/hritikraj8804/aws-142-serverless-databases-managing-tasks-with-amazon-dynamodb-2gh0)
