# Instruction

The Nautilus DevOps team is migrating a portion of its infrastructure to Azure. During the migration, they have created several virtual machines (VMs) in different regions. The team identified one VM that is experiencing increased workload demands and requires additional compute resources to maintain optimal performance.

Requirements:

1. Change the VM size from:

```text id="k7m2xp"
Standard_B1s
```

to:

```text id="u4q9lt"
Standard_B2s
```

for the virtual machine:

```text id="n8v3wa"
devops-vm
```

2. Ensure the VM is in the:

```text id="x5p1mr"
Running
```

state after the resize operation is complete.



# Solution

## **Steps to Complete the Task Using Azure Portal**

### **1. Login to Azure Portal**

Open:

```text id="w9m4qe"
https://portal.azure.com
```

Login using the provided credentials.



## **2. Open Virtual Machines Service**

In the Azure Portal search bar, search for:

```text id="b6x2nv"
Virtual Machines
```

Click:

```text id="r3k8yp"
Virtual Machines
```

from the services list.



## **3. Open the Existing VM**

Search and open the VM:

```text id="m1v7zd"
devops-vm
```

Verify the VM region is:

```text id="p8q4lx"
East US
```

<img width="1553" height="338" alt="vmbefore" src="https://github.com/user-attachments/assets/01435170-f75f-4761-a93a-6c8ce735ec59" />


## **4. Stop the Virtual Machine**

Azure may require the VM to be stopped before resizing.

From the VM overview page:

* Click:

```text id="t4p8qs"
Stop
```

Wait until the VM status changes to:

```text id="h2x6vn"
Stopped (deallocated)
```



## **5. Open the Size Configuration**

From the VM left-side menu:

Under:

```text id="f5n2wc"
Settings
```

Click:

```text id="y7r9mk"
Size
```



## **6. Change VM Size**

In the VM size list:

* Search for:

```text id="g1x7vq"
Standard_B2s
```

* Select:

```text id="d5m8ky"
Standard_B2s
```

Then click:

```text id="t2q4wc"
Resize
```

Wait for the resize operation to complete successfully.

<img width="1163" height="801" alt="size" src="https://github.com/user-attachments/assets/618de71c-205c-4e35-b46b-bc8b911b21fe" />


## **7. Start the Virtual Machine Again**

Return to the VM overview page.

Click:

```text id="n6p3rt"
Start
```

Wait until the VM state becomes:

```text id="g6p2wy"
Running
```



## **8. Verify the VM Size**

From the VM Overview page, confirm:

| Setting | Value          |
| ------- | -------------- |
| VM Name | `devops-vm`    |
| VM Size | `Standard_B2s` |
| Status  | `Running`      |

<img width="1547" height="311" alt="resized" src="https://github.com/user-attachments/assets/578f5b8e-8ac3-499f-8e29-42dfd5ea2b13" />


## VM Resize Summary

| Setting       | Value          |
| ------------- | -------------- |
| VM Name       | `devops-vm`    |
| Previous Size | `Standard_B1s` |
| New Size      | `Standard_B2s` |
| Region        | `East US`      |
| Final State   | `Running`      |



## Challenges

### **1. Resize Option Not Available**

Some VM sizes may not be available in all regions.

Ensure the VM is located in:

```text id="b1m6xz"
East US
```



### **2. VM Must Be Stopped**

If resize fails, stop the VM first until it reaches:

```text id="d9q4vt"
Stopped (deallocated)
```



### **3. Forgetting to Restart the VM**

After resizing, always click:

```text id="f2k7ya"
Start
```

to bring the VM back online.



### **4. Selecting Wrong Size**

Ensure the selected VM size is exactly:

```text id="c4t7yn"
Standard_B2s
```



## Fun Message

*"Your DevOps VM just leveled up with more Azure compute power ☁️⚡🚀"*
