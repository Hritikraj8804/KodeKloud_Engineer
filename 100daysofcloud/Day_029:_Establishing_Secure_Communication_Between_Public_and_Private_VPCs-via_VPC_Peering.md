# Instruction

The Nautilus DevOps team has been tasked with demonstrating the use of VPC Peering to enable communication between two VPCs. One VPC will be a private VPC that contains a private EC2 instance, while the other will be the default public VPC containing a publicly accessible EC2 instance.
1) There is already an existing EC2 instance in the public vpc/subnet:
Name: `datacenter-public-ec2`
2) There is already an existing Private VPC:
Name: datacenter-private-vpc
CIDR: `10.1.0.0/16`
3) There is already an existing Subnet in datacenter-private-vpc:
Name: datacenter-private-subnet
CIDR: `10.1.1.0/24`
4) There is already an existing EC2 instance in the private subnet:
Name: `datacenter-private-ec2`
5) Create a Peering Connection between the Default VPC and the Private VPC:
VPC Peering Connection Name: `datacenter-vpc-peering`
6) Configure Route Tables to enable communication between the two VPCs.
Ensure the private EC2 instance is accessible from the public EC2 instance.
7) Test the Connection:
Add `/root/.ssh/id_rsa.pub` public key to the public EC2 instance's ec2-user's authorized_keys to make sure we are able to ssh into this instance from AWS client host. You may also need to update the security group of the private EC2 instance to allow ICMP traffic from the `public/default` VPC CIDR. This will enable you to ping the private instance from the public instance.
SSH into the public EC2 instance and ensure that you can ping the private EC2 instance.

# Solution

Establishing a VPC Peering connection is like building a private bridge between two isolated islands. It allows resources in different VPCs to communicate using private IP addresses as if they were on the same network, without traffic ever touching the public internet.

Follow these steps to link the **Default VPC** and the **datacenter-private-vpc** and verify the connection.

---

### Step 1: Create the VPC Peering Connection

1. **Navigate to VPC Dashboard**: In the AWS Console, go to **VPC** > **Peering connections**.
2. **Create Connection**: Click **Create peering connection**.
* **Name**: `datacenter-vpc-peering`
* **VPC ID (Requester)**: Select the **Default VPC**.
* **VPC ID (Accepter)**: Select `datacenter-private-vpc`.

![1](https://github.com/user-attachments/assets/cf0a99db-4a20-4d86-84f0-3414c3708f88)

3. **Accept the Request**: The connection will initially be in a "Pending Acceptance" state. Select the connection, click **Actions**, and choose **Accept request**.

![2](https://github.com/user-attachments/assets/cb640ec8-e723-45fb-a279-67ea81db4c39)

### Step 2: Update Route Tables

For traffic to flow, both VPCs must know how to reach each other.

1. **Public VPC Route Table**:
* Find the route table associated with the Default VPC.
* Click **Edit routes** > **Add route**.
* **Destination**: `10.1.0.0/16` (Private VPC CIDR)
* **Target**: **Peering Connection** > Select `datacenter-vpc-peering`.


2. **Private VPC Route Table**:
* Find the route table associated with `datacenter-private-vpc`.
* Click **Edit routes** > **Add route**.
* **Destination**: Enter the CIDR of your **Default VPC** (typically `172.31.0.0/16`).
* **Target**: **Peering Connection** > Select `datacenter-vpc-peering`.

### Step 3: Update Security Groups

1. **Private EC2 Security Group**:
* Locate the Security Group attached to `datacenter-private-ec2`.
* **Add Inbound Rule**: Type **All ICMP - IPv4**.
* **Source**: Enter the CIDR block of the **Default VPC**.
* *This allows the "ping" signal to pass through.*



---

### Step 4: Test the Connection

1. **Set up SSH Access**:
* On your **AWS client host**, locate your public key (`/root/.ssh/id_rsa.pub`).

![3](https://github.com/user-attachments/assets/34fd475b-78c1-485b-97a7-a74f7626997e)

* Connect to the `datacenter-public-ec2` and append this key to the file at `~/.ssh/authorized_keys`.

2. **SSH into Public Instance**:
```bash
ssh ec2-user@<Public_EC2_IP>

```
![4](https://github.com/user-attachments/assets/3db42491-952b-4983-a65a-54dbea4dcaff)
![4 5](https://github.com/user-attachments/assets/5b6f1097-f415-4b96-ae71-53aa9eb77669)

3. **Ping Private Instance**: From the public instance terminal, ping the private IP of the private instance:
```bash
ping <Private_IP_of_datacenter-private-ec2>

```

![5](https://github.com/user-attachments/assets/ef49d43e-d10b-49b1-80a1-77db61eaff63)

---

### Key Technical Considerations

* **Transitive Peering**: Remember that VPC peering is not transitive. If VPC A is peered with B, and B is peered with C, A cannot talk to C through B.
* **Overlapping CIDRs**: Peering will fail if the two VPCs have overlapping IP address ranges.
* **DNS Resolution**: If you want to connect using hostnames (like `db.internal`) instead of IPs, you must enable "DNS Hostnames" and "DNS Support" in the peering connection options.

---

### Fun Fact

> VPC Peering is essentially "free" to set up. While you pay for the data transferred across the connection (at the same rate as intra-region data transfer), there is no hourly charge for the connection itself, unlike a Transit Gateway or VPN.

👉[Dev.to](https://dev.to/hritikraj8804/aws-129-bridging-the-gap-implementing-vpc-peering-akf)
