# Instruction

The Nautilus DevOps team has been tasked with preparing the infrastructure for a new Kubernetes-based application that will be deployed using Amazon EKS. The team is in the process of setting up an EKS cluster that meets their internal security and scalability standards. They require that the cluster be provisioned using the latest stable Kubernetes version to take advantage of new features and security improvements.

To minimize external exposure, the EKS cluster endpoint must be kept private. Additionally, the cluster needs to use the default VPC with availability zones `a`, `b`, and `c` to ensure high availability across different physical locations.

Your task is to create an EKS cluster named `xfusion-eks`, along with an IAM role for the cluster named `eksClusterRole`. The Kubernetes version must be `1.30`. Ensure that the cluster endpoint access is configured as private.

Finally, verify that the EKS cluster is successfully created with the correct configuration and is ready for workloads.

# Solution


### **Step 1: Create the IAM Role (`eksClusterRole`)**

EKS needs this role to manage AWS resources on your behalf.

1. Navigate to **IAM > Roles** and click **Create role**.
2. **Select trusted entity:** Choose **AWS Service**.
3. **Service or use case:** Select **EKS** from the dropdown, then select **EKS - Cluster**. Click **Next**.
4. **Add permissions:** The policy `AmazonEKSClusterPolicy` should be automatically attached. Click **Next**.
5. **Role name:** Enter `eksClusterRole`.
6. Click **Create role**.

![1](https://github.com/user-attachments/assets/db79bc0e-2710-4c07-9d3c-7cda2672a1e3)

---

### **Step 2: Create the EKS Cluster (`xfusion-eks`)**

1. Navigate to the **Amazon EKS** console and click **Add cluster > Create**.
2. **Configure cluster:**
* **Name:** `xfusion-eks`
* **Kubernetes version:** Select **1.30**.
* **Cluster service role:** Select the **eksClusterRole** you just created.
* Click **Next**.
3. **Specify networking:**
* **VPC:** Select your **default VPC**.
* **Subnets:** Ensure subnets representing Availability Zones **a, b, and c** are selected.
* **Cluster endpoint access:** 🚨 Select **Private**. (This disables public access and ensures the API server is only reachable within the VPC).
* Click **Next**.
4. **Configure logging:** Keep the defaults (disabled) or enable what you need, then click **Next**.
5. **Select add-ons:** Keep the default versions for CoreDNS, kube-proxy, and Amazon VPC CNI. Click **Next** until you reach the review page.
6. **Review and create:** Review your settings and click **Create**.

![2](https://github.com/user-attachments/assets/7f6a9ea7-1752-4a65-ab22-e58d06c55540)

---

### **Step 3: Verification**

Once the cluster status changes from **Creating** to **Active**:

1. Click on the cluster name **xfusion-eks**.
2. Under the **Overview** tab, verify the **Kubernetes version** is `1.30`.
3. Under the **Networking** tab, verify that **Cluster endpoint access** is set to **Private**.


![5 2](https://github.com/user-attachments/assets/7e77c7a5-1ea8-4fd9-a595-267400643172)

![5](https://github.com/user-attachments/assets/09e510ec-7990-4d9d-8cfd-1a4b7c2163fd)

![5 1](https://github.com/user-attachments/assets/0c4a27cd-a0f2-4232-9868-3910a9ecfb01)

---

👉[Dev.to](https://dev.to/hritikraj8804/aws-143-enterprise-kubernetes-provisioning-a-private-amazon-eks-cluster-3nn7)
