# Instruction

The Nautilus DevOps team has already set up a virtual machine and allocated a public IP address. The final task is to attach this public IP to the VM's network interface card (NIC).

Requirements:

* An existing VM named:

```text id="k7m2xp"
devops-vm-pip
```

already exists.

* An existing Public IP address named:

```text id="u4q9lt"
devops-pip
```

already exists.

* Attach the Public IP `devops-pip` to the network interface of the VM `devops-vm-pip`.
* Ensure the VM is properly assigned the public IP.

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

Search and open:

```text id="w9m4qe"
devops-vm-pip
```

<img width="1883" height="852" alt="vmlist" src="https://github.com/user-attachments/assets/82716020-8921-4794-92f0-62543f24a5bb" />


## **4. Open Networking Settings**

From the VM left-side menu:

Under:

```text id="b6x2nv"
Settings
```

Click:

```text id="r3k8yp"
Networking
```



## **5. Open the Network Interface (NIC)**

Under:

```text id="m1v7zd"
Network interface
```

click the attached NIC name.

This opens the NIC configuration page.



## **6. Open IP Configurations**

Inside the NIC settings:

* Under:

```text id="p8q4lx"
Settings
```

click:

```text id="t4p8qs"
IP configurations
```



## **7. Associate the Public IP Address**

Select the existing IP configuration, usually named:

```text id="h2x6vn"
ipconfig1
```

Under:

```text id="f5n2wc"
Public IP address
```

Click:

```text id="y7r9mk"
Associate
```

or select:

```text id="g1x7vq"
devops-pip
```

from the dropdown list.

Then click:

```text id="d5m8ky"
Save
```

<img width="1652" height="757" alt="apply" src="https://github.com/user-attachments/assets/0792a598-9a29-4f57-8f2b-86de835be8f3" />


## **8. Verify Public IP Attachment**

Return to the VM overview page.

Under:

```text id="t2q4wc"
Networking
```

verify that:

```text id="n6p3rt"
devops-pip
```

is associated with the VM NIC.

You should also see a public IP value displayed.

<img width="1497" height="347" alt="public ip" src="https://github.com/user-attachments/assets/301821aa-fa30-491d-8882-58a2de9fe4c5" />


## Public IP Attachment Summary

| Setting           | Value           |
| ----------------- | --------------- |
| VM Name           | `devops-vm-pip` |
| Public IP         | `devops-pip`    |
| Attachment Target | VM NIC          |
| Configuration     | `ipconfig1`     |



## Challenges

### **1. Wrong NIC Selected**

Ensure you modify the NIC attached to:

```text id="g6p2wy"
devops-vm-pip
```



### **2. Public IP Not Visible**

Verify the Public IP resource:

```text id="b1m6xz"
devops-pip
```

already exists in the same region and resource group.



### **3. Forgetting to Save Changes**

After associating the Public IP, always click:

```text id="d9q4vt"
Save
```



### **4. IP Configuration Selection**

Attach the Public IP under the correct configuration:

```text id="f2k7ya"
ipconfig1
```



## Fun Message

*"Your VM is now officially reachable from the Azure cloud highways ☁️🌍🚀"*
