# Instruction

The Nautilus datacenter team is migrating services to Azure. They are breaking down tasks to ensure better control and optimization. You are tasked with attaching an existing data disk to a virtual machine (VM).

Requirements:

* An existing VM named:

```text id="k7m2xp"
datacenter-vm
```

already exists.

* An existing managed disk named:

```text id="u4q9lt"
datacenter-disk
```

already exists.

* Attach the disk `datacenter-disk` to the VM `datacenter-vm` as a **data disk**.
* Ensure the disk is successfully attached.
* Make sure the VM initialization process has completed before submitting the task.

> **Note:** Use the Azure Portal (UI/Console) to complete this task.



# Solution

## **Steps to Complete the Task Using Azure Portal**

### **1. Login to Azure Portal**

Open:

```text id="n8v3wa"
https://portal.azure.com
```

Login using your Azure credentials.



## **2. Open Virtual Machines Service**

In the Azure Portal search bar, search for:

```text id="x5p1mr"
Virtual Machines
```

Click:

```text id="c2t7yk"
Virtual Machines
```

from the services list.



## **3. Open the Existing VM**

Search and open the VM:

```text id="w9m4qe"
datacenter-vm
```

<img width="1897" height="766" alt="vmlist" src="https://github.com/user-attachments/assets/051d3788-20a5-4d6a-be32-885c247ba69b" />


## **4. Wait for VM Initialization**

Ensure the VM status shows:

```text id="b6x2nv"
Running
```

Wait a few moments if the VM is still initializing.



## **5. Open the Disks Section**

Inside the VM menu:

* Under:

```text id="r3k8yp"
Settings
```

Click:

```text id="m1v7zd"
Disks
```



## **6. Attach the Existing Managed Disk**

Under the:

```text id="p8q4lx"
Data disks
```

section:

* Click:

```text id="t4p8qs"
Attach existing disks
```

<img width="1155" height="607" alt="disk form" src="https://github.com/user-attachments/assets/3353ce89-fb7a-42cc-a40b-f4e7e96676a0" />


## **7. Select the Managed Disk**

Choose the disk:

```text id="h2x6vn"
datacenter-disk
```

Keep the remaining settings as default.

Then click:

```text id="f5n2wc"
Apply
```

or

```text id="y7r9mk"
Save
```

depending on the Azure Portal version.



## **8. Verify Disk Attachment**

After saving:

* The disk:

```text id="g1x7vq"
datacenter-disk
```

should appear under:

```text id="d5m8ky"
Data disks
```

<img width="812" height="486" alt="disk" src="https://github.com/user-attachments/assets/19098837-0e4a-4b8b-bfdf-fd7da739f9fb" />


inside the VM configuration.



## Disk Attachment Summary

| Setting         | Value             |
| --------------- | ----------------- |
| VM Name         | `datacenter-vm`   |
| Disk Name       | `datacenter-disk` |
| Disk Type       | Managed Disk      |
| Attachment Type | Data Disk         |



## Challenges

### **1. VM Still Initializing**

Do not attach the disk while the VM provisioning status is still in progress.

Wait until the VM state becomes:

```text id="t2q4wc"
Running
```



### **2. Disk Not Visible**

Ensure both the VM and disk exist in the same region:

```text id="n6p3rt"
southcentralus
```



### **3. Selecting Wrong Disk Type**

Attach the disk under:

```text id="g6p2wy"
Data disks
```

and not as an OS disk.



### **4. Forgetting to Save Changes**

Always click:

```text id="b1m6xz"
Save
```

after attaching the disk.



## Fun Message

*"Your Nautilus VM just gained extra storage power in the Azure cloud ☁️💽🚀"*
