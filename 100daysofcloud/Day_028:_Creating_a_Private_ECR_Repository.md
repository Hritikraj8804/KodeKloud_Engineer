# Instruction

The Nautilus DevOps team has been tasked with setting up a containerized application. They need to create a private Amazon Elastic Container Registry (ECR) repository to store their Docker images. Once the repository is created, they will build a Docker image from a Dockerfile located on the aws-client host and push this image to the ECR repository. This process is essential for maintaining and deploying containerized applications in a streamlined manner.

Create a private ECR repository named `devops-ecr`. There is a Dockerfile under `/root/pyapp` directory on aws-client host, build a docker image using this Dockerfile and push the same to the newly created ECR repo, the image tag must be latest.

# Solution


### Step 1: Create the ECR Repository

You can create the repository via the AWS CLI. Run the following command:

```bash
aws ecr create-repository --repository-name devops-ecr

```
![1](https://github.com/user-attachments/assets/8ac651a7-2f96-4de7-aeeb-8716e01906af)

This will return a JSON response containing the `repositoryUri` (e.g., `123456789012.dkr.ecr.us-east-1.amazonaws.com/devops-ecr`). **Copy this URI**, as you will need it for tagging.

### Step 2: Authenticate Docker to ECR

Before you can push images, you must authenticate your local Docker client to the registry. Replace `REGION` with your actual AWS region (e.g., `us-east-1`):

```bash
aws ecr get-login-password --region REGION | docker login --username AWS --password-stdin YOUR_ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com

```

![2](https://github.com/user-attachments/assets/ca35a120-5fbb-442f-91da-5f656fb31f99)

If successful, you will see a `Login Succeeded` message.

### Step 3: Build the Docker Image

Navigate to the directory containing your Dockerfile and build the image:

```bash
cd /root/pyapp
docker build -t devops-ecr .

```
![3](https://github.com/user-attachments/assets/af41dfaa-0633-4936-b158-4870c442a91a)

### Step 4: Tag and Push the Image

Now, tag your local image so it matches the ECR repository URI and push it:

1. **Tag the image**:
```bash
docker tag devops-ecr:latest YOUR_ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com/devops-ecr:latest

```

2. **Push the image**:
```bash
docker push YOUR_ACCOUNT_ID.dkr.ecr.REGION.amazonaws.com/devops-ecr:latest

```

![4](https://github.com/user-attachments/assets/0d3c3b48-0dea-4fbd-a699-21754dd67378)

![5](https://github.com/user-attachments/assets/79b0f007-9b61-466d-b172-afdd654e2699)

---

### Key Technical Considerations

* **Permissions**: Ensure the `aws-client` host has an IAM role or credentials with `ecr:CreateRepository`, `ecr:GetAuthorizationToken`, and `ecr:PutImage` permissions.
* **The Dot (`.`)**: In the `docker build` command, the `.` at the end is crucial; it tells Docker to look for the Dockerfile in the *current* directory.
* **Regionality**: ECR repositories are regional. Always ensure your login command and your tag use the same region where the repository was created.

---

### Fun Fact

> Pushing an image to ECR is like checking a suitcase at the airport. You "tag" it with your destination (the URI), and once it's "pushed" onto the conveyor belt, it's stored safely until your application (the plane) is ready to pick it up at the other end!

👉[Dev.to](https://dev.to/hritikraj8804/aws-128-setting-up-amazon-ecr-and-pushing-docker-images-35i4)
