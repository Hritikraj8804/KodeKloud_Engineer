# Instruction

The Nautilus Development Team is preparing for a major update to their database infrastructure. To ensure a smooth transition and to safeguard data, the team has requested the DevOps team to take a snapshot of the current RDS instance and restore it to a new instance. This process is crucial for testing and validation purposes before the update is rolled out to the production environment. The snapshot will serve as a backup, and the new instance will be used to verify that the backup process works correctly and that the application can function seamlessly with the restored data.
As a member of the Nautilus DevOps Team, your task is to perform the following:

Take a Snapshot: Take a snapshot of the `devops-rds` RDS instance and name it `devops-snapshot` (please wait devops-rds instance to be in `available` state).

Restore the Snapshot: Restore the snapshot to a new RDS instance named `devops-snapshot-restore`.

Configure the New RDS Instance: Ensure that the new RDS instance has a class of `db.t3.micro`.

Verify the New RDS Instance: The new RDS instance must be in the Available state upon completion of the restoration process.

# Solution

### **Step 1: Create the Manual Snapshot**

Before restoring, you must capture the state of your existing database.

1. Navigate to the **RDS Console** and select **Databases**.
![1](https://github.com/user-attachments/assets/05f0d856-9750-4eef-92c6-d64903d0ad49)

2. Ensure `devops-rds` is in the **Available** state.
![2](https://github.com/user-attachments/assets/53a5e50c-e908-436e-ab1b-7fc26a6128b4)

3. Select `devops-rds`, click the **Actions** button, and choose **Take snapshot**.
![3](https://github.com/user-attachments/assets/b6658286-a52e-41fb-9029-671f493ebc05)

4. **Snapshot Name**: `devops-snapshot`.
5. Click **Take snapshot** and wait for the status to change from *Creating* to *Available* in the **Snapshots** tab.
![4](https://github.com/user-attachments/assets/d30f8395-2007-4cff-bd79-3744bd47b96d)
![5](https://github.com/user-attachments/assets/06ebd1d0-57b0-4440-b8c7-0a3b20b08708)

---

### **Step 2: Restore Snapshot to New Instance**

Once the snapshot is ready, you will use it to "clone" the database into a new instance.

1. Go to the **Snapshots** section in the left sidebar.
2. Select `devops-snapshot`.
3. Click **Actions** > **Restore snapshot**.
![6](https://github.com/user-attachments/assets/7a3e87ad-ac7a-4030-8ed4-a09f3d8e3d5d)

4. **Settings**:
* **DB instance identifier**: `devops-snapshot-restore`.


5. **Instance Configuration**:
* **DB instance class**: Select **Burstable classes** and choose **db.t3.micro**.


6. **Connectivity**:
* Ensure the VPC and Subnet settings match your previous private configuration (Public access: **No**).


7. Scroll to the bottom and click **Restore DB instance**.
![7](https://github.com/user-attachments/assets/31d0787f-2ae0-44e1-aa12-381ed988e902)

---

### **Step 3: Verification and Validation**

Restoring an instance typically takes longer than creating a fresh one because AWS must pull the data blocks from S3 to the new EBS volumes.
![8](https://github.com/user-attachments/assets/8cfef6c6-ec8c-439c-860b-14415b5e82c1)

---

| Checkpoint | Expected Result |
| --- | --- |
| **Instance Name** | `devops-snapshot-restore` |
| **Instance Class** | `db.t3.micro` |
| **Status** | **Available** |
| **Data Integrity** | Matches the state of `devops-rds` at the time of snapshot. |

---

> **DevOps Pro-Tip:** When restoring for testing purposes, remember that the restored instance will have its own **Endpoint**. You will need to provide this new endpoint string to the development team so they can point their test application to the restored data rather than the production database.

---

### **Final Status Check**

Monitor the **Databases** dashboard. Once the status for `devops-snapshot-restore` turns green and says **Available**, your task is complete and the environment is ready for the development team’s validation tests.

👉[Dev.to](https://dev.to/hritikraj8804/aws-132-data-time-travel-rds-snapshots-and-restoration-34eg)
