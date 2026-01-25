# Instruction

The Nautilus DevOps team is tasked with enabling internet access for an EC2 instance running in a private subnet. This instance should be able to upload a test file to a public S3 bucket once it can access the internet. To achieve this, the team must set up a NAT Gateway in a public subnet within the same VPC.

1) A VPC named `nautilus-priv-vpc` and a private subnet `nautilus-priv-subnet` have already been created.

2) An EC2 instance named nautilus-priv-ec2 is already running in the private subnet.

3) The EC2 instance is configured with a cron job that uploads a test file to a bucket `nautilus-nat-18582` once internet is accessible.

Your task is to:

Create a public subnet named `nautilus-pub-subnet` in the same VPC.

Create an Internet Gateway and attach it to the VPC.

Create a route table `nautilus-pub-rt` and associate it with the public subnet.

Allocate an Elastic IP and create a NAT Gateway named `nautilus-natgw`.

Update the private route table to route `0.0.0.0/0` traffic via the NAT Gateway.

Once complete, verify that the EC2 instance can reach the internet by confirming the presence of the test file in the S3 bucket `nautilus-nat-18582`. After completing all the configuration, please wait a few minutes for the test file to appear in the bucket, as it may take 2–3 minutes.

# Solution

---

### **Step 1: Create the Public Subnet**

1. Go to **VPC > Subnets > Create subnet**.
2. **VPC ID:** Select `nautilus-priv-vpc`.
3. **Subnet name:** `nautilus-pub-subnet`.
4. **IPv4 CIDR block:** Use a range like `10.0.2.0/24` (ensure it doesn't overlap with the private one).
5. Click **Create subnet**.

### **Step 2: Internet Gateway (IGW)**

1. Go to **Internet Gateways > Create internet gateway**.
2. **Name:** `nautilus-igw`.
3. Once created, select it and click **Actions > Attach to VPC**.
4. Select `nautilus-priv-vpc` and click **Attach**.

### **Step 3: Public Route Table & Association**

1. Go to **Route Tables > Create route table**.
2. **Name:** `nautilus-pub-rt`. **VPC:** `nautilus-priv-vpc`.
3. **Add Route:** Click the **Routes** tab > **Edit routes**. Add `0.0.0.0/0` with the target as your **Internet Gateway** (`nautilus-igw`).
4. **ASSOCIATE:** Click the **Subnet associations** tab > **Edit subnet associations**. Select **`nautilus-pub-subnet`**. **(Crucial step)**.

### **Step 4: NAT Gateway**

1. Go to **NAT Gateways > Create NAT gateway**.
2. **Name:** `nautilus-natgw`.
3. **Subnet:** 🚨 **Select `nautilus-pub-subnet**` (The one you just created).
4. **Elastic IP:** Click **Allocate Elastic IP**.
5. Click **Create NAT gateway**. Wait for it to show as **Available**.

### **Step 5: Update & Associate Private Route Table**


1. Go to **Route Tables > Create route table** (or find the existing one for the private subnet).
2. **Name it:** `nautilus-priv-rt` for clarity.
3. **Add Route:** Click **Edit routes**. Add `0.0.0.0/0` with the target as your **NAT Gateway** (`nautilus-natgw`).
4. ** Click the **Subnet associations** tab > **Edit subnet associations**. Select **`nautilus-priv-subnet`**.
* *Even if it says it's "implicitly" associated with a main table, you must make it an **explicit association** for the lab to pass.*

---

### **Step 6: Verification**

1. **Wait 3 minutes** for the cron job to run.
2. Go to **S3 > Buckets > nautilus-nat-18582**.
3. Look for the test file.


To verify your work using the CLI for this new lab environment, we will first gather the correct IDs for the `nautilus` infrastructure and then confirm the routing and S3 file status.

```bash
# Get the VPC ID for nautilus-priv-vpc
VPC_ID=$(aws ec2 describe-vpcs --filters "Name=tag:Name,Values=nautilus-priv-vpc" --query "Vpcs[0].VpcId" --output text)

# Get the Subnet ID for nautilus-priv-subnet
PRIV_SUBNET_ID=$(aws ec2 describe-subnets --filters "Name=tag:Name,Values=nautilus-priv-subnet" --query "Subnets[0].SubnetId" --output text)

# Get the Subnet ID for the NEW nautilus-pub-subnet
PUB_SUBNET_ID=$(aws ec2 describe-subnets --filters "Name=tag:Name,Values=nautilus-pub-subnet" --query "Subnets[0].SubnetId" --output text)

echo "VPC: $VPC_ID | Private Subnet: $PRIV_SUBNET_ID | Public Subnet: $PUB_SUBNET_ID"
```

---

### **Step 2: Verify the Private Route Table & Association**

This is the step that failed previously. Let's check if the private subnet is **explicitly associated** with a route table that has a path to the NAT Gateway.

```bash
# Find the Route Table associated with the private subnet
RTB_ID=$(aws ec2 describe-route-tables --filters "Name=association.subnet-id,Values=$PRIV_SUBNET_ID" --query "RouteTables[0].RouteTableId" --output text)

# 1. Check if the association exists
aws ec2 describe-route-tables --route-table-ids $RTB_ID --query "RouteTables[0].Associations[?SubnetId=='$PRIV_SUBNET_ID']"

# 2. Check if the 0.0.0.0/0 route points to the NAT Gateway
aws ec2 describe-route-tables --route-table-ids $RTB_ID --query "RouteTables[0].Routes[?DestinationCidrBlock=='0.0.0.0/0']"
```

* **Success Criteria:** The first command should return an association block (not `[]`), and the second should show your `NatGatewayId`.

---

### **Step 3: Verify the S3 Test File**

Wait **2–3 minutes** for the cron job to fire, then run:

```bash
# List objects in the new bucket
aws s3 ls s3://nautilus-nat-18582/

# Verify the specific file exists (Key might be datacenter-test.txt or similar)
aws s3api head-object --bucket nautilus-nat-18582 --key datacenter-test.txt
```

---

### **🚨 The "Insurance" Command**

If Step 2 showed an empty association `[]`, run this immediately to fix it:

```bash
aws ec2 associate-route-table --route-table-id $RTB_ID --subnet-id $PRIV_SUBNET_ID
```

---

👉[Dev.to](https://dev.to/hritikraj8804/aws-145-private-subnet-internet-access-implementing-nat-gateways-1e0a)
