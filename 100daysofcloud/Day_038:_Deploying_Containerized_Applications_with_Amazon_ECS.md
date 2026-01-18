# Instruction

The Nautilus DevOps team is tasked with deploying a containerized application using Amazon's container services. They need to create a private Amazon Elastic Container Registry (ECR) to store their Docker images and use Amazon Elastic Container Service (ECS) to deploy the application. The process involves building a Docker image from a given Dockerfile, pushing it to the ECR, and then setting up an ECS cluster to run the application.

Create a Private ECR Repository:

Create a private ECR repository named `devops-ecr` to store Docker images.

Build and Push Docker Image:

Use the Dockerfile located at `/root/pyapp` on the `aws-client` host.

Build a Docker image using this Dockerfile.

Tag the image with latest tag.

Push the Docker image to the `devops-ecr` repository.

Create and Configure ECS cluster:

Create an ECS cluster named `devops-cluster` using the Fargate launch type.

Create an ECS Task Definition:

Define a task named `devops-taskdefinition` using the Docker image from the devops-ecr ECR repository.

Specify necessary CPU and memory resources.

Deploy the Application Using ECS Service:

Create a service named `devops-service` on the `devops-cluster` to run the task.

Ensure the service runs at least one task.

# Solution


### **Step 1: Create a Private ECR Repository**

First, you need a place to store your container images.

1. Navigate to the **Amazon ECR** console.
2. Choose **Repositories** from the left sidebar and click **Create repository**.
3. **Visibility settings**: Private.
4. **Repository name**: `devops-ecr`.
5. Keep all other settings as default and click **Create repository**.

---

### **Step 2: Build and Push the Docker Image**

Perform these steps on the `aws-client` host where the Dockerfile is located.

1. **Navigate to the directory**:
```bash
cd /root/pyapp
```


2. **Authenticate Docker to ECR**:
Retrieve the login password and authenticate your Docker client. Replace `<region>` and `<aws_account_id>` with your actual values.
```bash
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```

![2](https://github.com/user-attachments/assets/428c7167-26ef-4bcf-8d93-3b5ebd1938a1)

3. **Build the Image**:
```bash
docker build -t devops-ecr .
```

4. **Tag the Image**:
```bash
docker tag devops-ecr:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/devops-ecr:latest
```

![3](https://github.com/user-attachments/assets/aaa37d1d-892d-4b23-9363-189d1f70fe24)

5. **Push the Image**:
```bash
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/devops-ecr:latest
```

![4](https://github.com/user-attachments/assets/54f7fdaf-ecc3-45a0-9c97-22eabd1cc2ea)

---

### **Step 3: Create ECS Cluster and Task Definition**

#### **1. Create the Cluster**

1. Navigate to **Amazon ECS > Clusters > Create cluster**.
2. **Cluster name**: `devops-cluster`.
3. **Infrastructure**: Ensure **AWS Fargate (serverless)** is selected.
4. Click **Create**.

![5](https://github.com/user-attachments/assets/a2f11a26-c7b7-44e2-99f8-affc5aac4e39)

#### **2. Create Task Definition**

1. In the ECS sidebar, go to **Task Definitions > Create new task definition**.
2. **Task definition family**: `devops-taskdefinition`.
3. **Infrastructure requirements**: Launch type **AWS Fargate**.
4. **Task size**:
* **CPU**: `.25 vCPU` (minimum for testing).
* **Memory**: `.5 GB`.
5. **Container details**:
* **Name**: `pyapp-container`.
* **Image URI**: Paste the URI of the image you just pushed to `devops-ecr`.
6. Click **Create**.

![6](https://github.com/user-attachments/assets/a51fa19a-45b5-4090-aa05-22b55bdc96e3)

---

### **Step 4: Deploy the ECS Service**

1. Go back to your `devops-cluster`.
2. In the **Services** tab, click **Create**.
3. **Deployment configuration**:
* **Application type**: Service.
* **Family**: `devops-taskdefinition` (select the latest revision).
* **Service name**: `devops-service`.
* **Desired tasks**: `1`.
4. **Networking**: Ensure it is deployed into a subnet with access to ECR (usually requires a NAT Gateway or VPC Endpoints if in a private subnet).
5. Click **Create**.

![7](https://github.com/user-attachments/assets/5ff6283f-6e63-4329-b67f-63861c76e483)

---

### **Step 5: Verification**

Monitor the service in the ECS console. Once the **Last status** of the task changes from `PROVISIONING` or `PENDING` to `RUNNING`, your application is successfully deployed.

| Component | Status Check |
| --- | --- |
| **ECR** | Image visible in `devops-ecr` with `latest` tag. |
| **ECS Task** | Status is **Running**. |
| **ECS Service** | Running count matches Desired count (1). |

![8](https://github.com/user-attachments/assets/c903a00c-95ce-494f-bbd1-3785a0c22b92)

👉[Dev.to](https://dev.to/hritikraj8804/aws-138-container-orchestration-deploying-apps-with-amazon-ecs-fargate-b2a)
