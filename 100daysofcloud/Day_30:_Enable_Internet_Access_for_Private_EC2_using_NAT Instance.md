# Instruction

The Nautilus DevOps team is tasked with enabling internet access for an EC2 instance running in a private subnet. This instance should be able to upload a test file to a public S3 bucket once it can access the internet. To minimize costs, the team has decided to use a NAT Instance instead of a NAT Gateway.
The following components already exist in the environment:
1) A VPC named `datacenter-priv-vpc` and a private subnet named `datacenter-priv-subnet` have been created.
2) An EC2 instance named `datacenter-priv-ec2` is already running in the private subnet.
3) The EC2 instance is configured with a cron job that uploads a test file to the S3 bucket `datacenter-nat-31923` every minute. Upload will only succeed once internet access is established.
Your task is to:

Create a new public subnet named datacenter-pub-subnet in the existing VPC.

Launch a NAT Instance in the public subnet using an Amazon Linux 2 AMI and name it `datacenter-nat-instance`. Configure this instance to act as a NAT instance. Make sure to use a custom security group for this instance.

After the configuration, verify that the test file `datacenter-test.txt` appears in the S3 bucket `datacenter-nat-31923`. This indicates successful internet access from the private EC2 instance via the NAT Instance.

# Solution

---

### Step 1: Create the Public Subnet

1. **Navigate** to the VPC Dashboard and select **Subnets**.
2. **Create Subnet**:
* **VPC ID**: Select `datacenter-priv-vpc`.
* **Subnet name**: `datacenter-pub-subnet`.
* **CIDR block**: Choose a range that does not overlap with your private subnet (e.g., `10.0.1.0/24`).


3. **Attach Internet Gateway**: Ensure the route table for this new public subnet has a route () pointing to an **Internet Gateway (IGW)**. This is what makes it "public."

---

### Step 2: Configure the NAT Instance Security Group

Create a custom security group specifically for the NAT instance:

* **Inbound Rules**:
* **HTTP (80)** and **HTTPS (443)**: Source should be the **CIDR of the private subnet** (`datacenter-priv-subnet`).


* **Outbound Rules**:
* Allow all traffic () so the instance can reach the internet to forward requests.



---

### Step 3: Launch and Configure the NAT Instance

1. **Launch Instance**: Use an **Amazon Linux 2 AMI** (t3.nano or t3.micro is sufficient for costs).
* **Name**: `datacenter-nat-instance`.
* **Network**: Place it in `datacenter-pub-subnet` and enable **Auto-assign Public IP**.
* **Security Group**: Select the custom group created in Step 2.


2. **Disable Source/Destination Check**: This is the most critical step.
* Select the instance > **Actions** > **Networking** > **Change source/destination check**.
* Set this to **Stop/Disable**. (This allows the instance to handle traffic that isn't specifically destined for its own IP).


3. **Configure NAT Software**: SSH into the NAT instance and run the following to enable IP forwarding and masquerading:
```bash
# Enable IP forwarding in the kernel
echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/custom-ip-forwarding.conf
sudo sysctl -p /etc/sysctl.d/custom-ip-forwarding.conf

# Configure iptables to masquerade traffic
sudo yum install iptables-services -y
sudo systemctl enable iptables
sudo systemctl start iptables
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo service iptables save

```



---

### Step 4: Update the Private Route Table

To direct traffic from the private instance to the internet:

1. Find the **Route Table** associated with `datacenter-priv-subnet`.
2. **Edit Routes** > **Add Route**:
* **Destination**: `0.0.0.0/0`
* **Target**: **Instance** > Select `datacenter-nat-instance`.



---

### Step 5: Verification

Wait a few minutes for the cron job on `datacenter-priv-ec2` to run.

* Go to the **S3 Console** and open the `datacenter-nat-31923` bucket.
* Confirm that `datacenter-test.txt` has been uploaded.

### Key Technical Considerations

* **Instance Reliability**: Unlike a NAT Gateway, if your NAT instance stops or crashes, the private subnet loses internet access. In production, you'd usually use an Auto Scaling Group to replace it if it fails.
* **Performance**: A small instance like a `t3.nano` has limited network bandwidth. If you expect high traffic, a NAT Gateway is often more cost-effective when factoring in engineering time.


👉[Dev.to](https://dev.to/hritikraj8804/aws-130-routing-the-private-way-implementing-a-nat-instance-1c9j)
