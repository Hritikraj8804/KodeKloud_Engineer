# Instruction

The Nautilus DevOps team needs to expand the storage capacity of an existing virtual machine and add an additional data disk to support increased workloads.

### Requirements

#### Existing VM

```text
xfusion-vm
```

### Tasks

1. Expand the existing OS disk from:

```text
32 GiB → 64 GiB
```

2. Create a new managed disk:

| Setting | Value                |
| ------- | -------------------- |
| Name    | `xfusion-disk`       |
| Size    | `64 GiB`             |
| Type    | `Standard HDD (LRS)` |

3. Attach the new disk to:

```text
xfusion-vm
```

4. Mount the new disk inside the VM at:

```text
/mnt/xfusion-disk
```

> **Note:** Use the Azure Portal (UI) for Azure resource changes and SSH into the VM for disk formatting/mounting.



# Solution

## Part 1: Expand the Existing OS Disk

### 1. Login to Azure Portal

Open:

```text
https://portal.azure.com
```



### 2. Open the VM

Search:

```text
Virtual Machines
```

Open:

```text
xfusion-vm
```

<img width="1176" height="721" alt="vm before" src="https://github.com/user-attachments/assets/4b4e4a5e-0545-4efa-b1c7-3031f54835ef" />

### 3. Open the OS Disk

Navigate to:

```text
Settings → Disks
```

Click the OS disk name.



### 4. Resize the OS Disk

Click:

```text
Size + Performance
```

Change:

```text
32 GiB
```

to:

```text
64 GiB
```

<img width="1515" height="841" alt="size select" src="https://github.com/user-attachments/assets/31b705dd-5d00-49aa-b72d-e6b6bc5e6fb3" />

Click:

```text
Save
```

Wait for the update to complete.

<img width="1552" height="662" alt="size update" src="https://github.com/user-attachments/assets/1ce82771-8142-48d7-ba20-e21e110245dc" />


# Part 2: Create the New Managed Disk

### 5. Create a Disk

Search:

```text
Disks
```

Click:

```text
+ Create
```



### 6. Configure the Disk

| Setting      | Value                |
| ------------ | -------------------- |
| Name         | `xfusion-disk`       |
| Source Type  | `None (empty disk)`  |
| Storage Type | `Standard HDD (LRS)` |
| Size         | `64 GiB`             |

Click:

```text
Review + Create
```

Then:

```text
Create
```

<img width="976" height="847" alt="disk form" src="https://github.com/user-attachments/assets/9f1ce160-0a6f-4674-b723-42a3cc078dfa" />


# Part 3: Attach Disk to VM

### 7. Open VM Disks

Return to:

```text
xfusion-vm
```

Navigate:

```text
Settings → Disks
```

<img width="1317" height="477" alt="disk list" src="https://github.com/user-attachments/assets/7ab3d61b-ed8a-4ca8-9f5f-efdb7e981503" />


### 8. Attach Existing Disk

Under:

```text
Data disks
```

Click:

```text
Attach existing disks
```

Select:

```text
xfusion-disk
```

Click:

```text
Save
```

Wait until attachment completes.

<img width="826" height="367" alt="attaching" src="https://github.com/user-attachments/assets/f0c56263-47d9-4517-b747-67d0176f0bff" />


# Part 4: Mount the Disk Inside the VM

### 9. SSH into the VM

Copy the VM Public IP and connect:

```bash
ssh azureuser@<PUBLIC_IP>
```



### 10. Identify the New Disk

```bash
lsblk
```

Example output:

```text
sda   64G
sdb   64G
```

Typically:

```text
sdb
```

will be the new disk.

Verify:

```bash
sudo fdisk -l
```



### 11. Create a Partition

Replace `sdb` if your disk name differs.

```bash
sudo parted /dev/sdb --script mklabel gpt
sudo parted /dev/sdb --script mkpart primary ext4 0% 100%
```



### 12. Format the Disk

```bash
sudo mkfs.ext4 /dev/sdb1
```



### 13. Create Mount Point

```bash
sudo mkdir -p /mnt/xfusion-disk
```



### 14. Mount the Disk

```bash
sudo mount /dev/sdb1 /mnt/xfusion-disk
```

Verify:

```bash
df -h
```

Expected:

```text
/mnt/xfusion-disk
```

appears in the output.



### 15. Make Mount Persistent

Get UUID:

```bash
sudo blkid /dev/sdb1
```

Example:

```text
UUID="abcd-1234"
```

Edit:

```bash
sudo vi /etc/fstab
```

Add:

```text
UUID=abcd-1234 /mnt/xfusion-disk ext4 defaults,nofail 0 2
```

Save.

Test:

```bash
sudo mount -a
```

No errors should appear.

<img width="681" height="792" alt="disk mount" src="https://github.com/user-attachments/assets/433d0160-87d9-42e6-9c2d-ba0918138478" />


# Verification Checklist

### Azure

✅ VM:

```text
xfusion-vm
```

✅ OS Disk:

```text
64 GiB
```

✅ Data Disk:

```text
xfusion-disk
```

✅ Data Disk Size:

```text
64 GiB
```

✅ Storage Type:

```text
Standard HDD (LRS)
```



### Linux

✅ Disk attached

```bash
lsblk
```

shows the new disk.

✅ Mounted at:

```text
/mnt/xfusion-disk
```

✅ Persistent after reboot via:

```text
/etc/fstab
```



## Common Lab Issue

Many KodeKloud validators check the mount point itself. Verify:

```bash
mount | grep xfusion-disk
```

returns:

```text
/mnt/xfusion-disk
```

before submitting.



## Fun Message

*"Your xFusion VM now has double the OS storage and a brand-new 64 GiB data disk ready for action ☁️💽🚀"*
