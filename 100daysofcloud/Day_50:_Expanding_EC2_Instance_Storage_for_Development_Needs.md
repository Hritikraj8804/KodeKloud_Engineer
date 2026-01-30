# Instruction

The Nautilus DevOps Team has recently been informed by the Development Team that their EC2 instance is running out of storage space. This instance, crucial for development activities, is named datacenter-ec2 and currently has an attached volume of 8 GiB. To accommodate the increasing data requirements, the storage needs to be expanded to 12 GiB. This change should ensure that the expanded space is immediately available for use within the instance without disrupting ongoing activities.

Identify Volume: Find the volume attached to the `datacenter-ec2` instance.

Expand Volume: Increase the volume size from `8 GiB` to `12 GiB`.

Reflect Changes: Ensure the root (`/`) partition within the instance reflects the expanded size from `8 GiB` to `12 GiB`.

SSH Access: Use the key pair located at `/root/datacenter-keypair.pem` on the aws-client host to SSH into the EC2 instance.

# Solution

---
### **Step 1: Identify the Volume**

First, we need the Volume ID attached to `datacenter-ec2`.

```bash
# Get the Volume ID
VOL_ID=$(aws ec2 describe-instances \
    --filters "Name=tag:Name,Values=datacenter-ec2" \
    --query "Reservations[0].Instances[0].BlockDeviceMappings[0].Ebs.VolumeId" \
    --output text)

echo "Volume ID: $VOL_ID"
```

![1](https://github.com/user-attachments/assets/7f9f0d60-7f30-4cea-93e4-010c84a3b961)

---

### **Step 2: Expand the Volume (8 GiB to 12 GiB)**

We will modify the volume size. Note that AWS allows you to increase size without detaching the volume.

```bash
aws ec2 modify-volume --volume-id $VOL_ID --size 12
```

![3](https://github.com/user-attachments/assets/b9b285e8-b8fd-4a71-86cb-9584ec428651)

> **Note:** It may take a few seconds for the "optimization" to reach a state where the OS can see the new capacity.

---

### **Step 3: SSH and Extend the Partition**

Now we need to tell the Linux OS to actually use that extra 4 GiB.

1. **Get the Public IP:**
```bash
INST_IP=$(aws ec2 describe-instances --filters "Name=tag:Name,Values=datacenter-ec2" --query "Reservations[0].Instances[0].PublicIpAddress" --output text)
```


2. **SSH into the instance:**
```bash
ssh -i /root/datacenter-keypair.pem ubuntu@$INST_IP
```


3. **Check the current layout:**
Inside the instance, run `lsblk`. You will likely see that the disk (`nvme0n1` or `xvda`) is 12G, but the partition (`nvme0n1p1` or `xvda1`) is still 8G.

![4](https://github.com/user-attachments/assets/46927acc-491c-487f-8028-629557ea5101)

5. **Grow the Partition and Filesystem:**
Run these commands inside the EC2 instance:

```bash
# Grow the partition (usually partition 1)
# Identify your disk first with lsblk. Assuming it's /dev/nvme0n1
sudo growpart /dev/nvme0n1 1
```

![5](https://github.com/user-attachments/assets/1fb82c9d-1d3b-4fe0-aace-e14f13dbded0)

```bash
# Resize the filesystem (ext4)
sudo resize2fs /dev/nvme0n1p1

# Or if it's XFS (common in RHEL/Amazon Linux):
# sudo xfs_growfs -d /
```

![6](https://github.com/user-attachments/assets/c00eb213-abc2-43a3-bec4-3df3aefd7346)

---

### **Step 4: Verify**

Run `df -h /` to confirm the root partition now shows **12G**.

![7](https://github.com/user-attachments/assets/e80cd25f-08f7-4c4d-b1e1-af0cad799d8f)

![8](https://github.com/user-attachments/assets/a5ab9a80-9bfc-44b2-a540-c5e46fc52f08)


## Docs:
[Extend the file system after resizing an Amazon EBS volume](https://docs.aws.amazon.com/ebs/latest/userguide/recognize-expanded-volume-linux.html)

👉[Dev.to](https://dev.to/hritikraj8804/aws-150-zero-downtime-storage-scaling-expanding-ebs-volumes-1oik)
