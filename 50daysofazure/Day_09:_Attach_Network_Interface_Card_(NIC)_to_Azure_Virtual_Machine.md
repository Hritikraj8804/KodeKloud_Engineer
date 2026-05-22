# Instruction

The DevOps team is migrating services to Azure and is performing the migration in smaller phases for better management and optimization.

Requirements:

* An existing VM named:

```text id="k7m2xp"
devops-vm
```

already exists.

* An existing Network Interface (NIC) named:

```text id="u4q9lt"
devops-nic
```

already exists.

* Attach the NIC `devops-nic` to the VM `devops-vm`.
* Ensure the NIC status becomes:

```text id="n8v3wa"
Attached
```

before submitting the task.

* Ensure VM initialization is fully completed before performing the attachment.

> **Note:** Use the Azure Portal (UI/Console) to complete this task.



# Solution

## **Steps to Complete the Task Using Azure Portal**

### **1. Login to Azure Portal**

Open:

```text id="x5p1mr"
https://portal.azure.com
```

Login using your Azure credentials.



## **2. Open Virtual Machines Service**

In the Azure Portal search bar, search for:

```text id="c2t7yk"
Virtual Machines
```

Click:

```text id="w9m4qe"
Virtual Machines
```

from the services list.



## **3. Open the Existing VM**

Search and open:

```text id="b6x2nv"
devops-vm
```



## **4. Verify VM Initialization**

Ensure the VM status shows:

```text id="r3k8yp"
Running
```

Wait until provisioning and initialization are fully completed.

<img width="1907" height="787" alt="vm" src="https://github.com/user-attachments/assets/f0e7f743-187c-4a1a-a05f-82dd4eaa00ee" />


## **5. Stop (Deallocate) the VM**

Azure requires the VM to be stopped before attaching another NIC.

Inside the VM overview page:

* Click:

```text id="m1v7zd"
Stop
```

Wait until the VM status changes to:

```text id="p8q4lx"
Stopped (deallocated)
```



## **6. Open Networking Settings**

In the VM left-side menu:

Under:

```text id="t4p8qs"
Settings
```

Click:

```text id="h2x6vn"
Networking
```



## **7. Attach the Existing NIC**

Under:

```text id="f5n2wc"
Network interfaces
```

Click:

```text id="y7r9mk"
Attach network interface
```

Select the NIC:

```text id="g1x7vq"
devops-nic
```

Then click:

```text id="d5m8ky"
OK
```

or

```text id="t2q4wc"
Attach
```

depending on the Azure Portal version.

<img width="817" height="630" alt="attach" src="https://github.com/user-attachments/assets/c1e4b1c2-e758-4a97-977e-b183b58f4c8a" />


## **8. Start the Virtual Machine Again**

Return to the VM overview page.

Click:

```text id="n6p3rt"
Start
```

Wait until the VM status becomes:

```text id="g6p2wy"
Running
```



## **9. Verify NIC Attachment**

Go back to:

```text id="b1m6xz"
Networking
```

Verify the NIC:

```text id="d9q4vt"
devops-nic
```

appears in the attached network interfaces list.

Its status should show:

```text id="f2k7ya"
Attached
```



## NIC Attachment Summary

| Setting  | Value              |
| -------- | ------------------ |
| VM Name  | `devops-vm`        |
| NIC Name | `devops-nic`       |
| Region   | `South Central US` |
| Status   | `Attached`         |



## Challenges

### **1. VM Not Deallocated**

Azure does not allow attaching a secondary NIC while the VM is running.

Always stop the VM first until the state becomes:

```text id="c4t7yn"
Stopped (deallocated)
```



### **2. NIC Not Visible**

Ensure the NIC and VM are in the same region:

```text id="q2v6mk"
southcentralus
```

and in the same virtual network if required.



### **3. Forgetting to Start the VM Again**

After attaching the NIC, restart the VM by clicking:

```text id="z5x1wp"
Start
```



### **4. VM Size Limitation**

Some VM sizes support only one NIC.

If attachment fails, verify the VM size supports multiple NICs.



## Fun Message

*"Your DevOps VM just gained another network lane in Azure cloud traffic ☁️🌐🚀"*
