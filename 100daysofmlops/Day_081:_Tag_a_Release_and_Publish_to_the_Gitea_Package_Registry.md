# Instruction

The xFusionCorp Industries ML Platform team wants every tagged release of the **fraud-detector** project to automatically:

* Build the production Docker image
* Push the image to Gitea's integrated Container Registry
* Generate `metrics.json`
* Attach `metrics.json` to the Git release

The release workflow was already configured, but two steps were left as **TODOs**:

* **Build image**
* **Push image to Gitea registry**

Your task was to complete these steps, then create the **v0.1.0** release so the workflow publishes the container image automatically.

# Solution Steps

## Step 1

Open the repository.

```bash
cd /root/code/fraud-detector
```

## Step 2

Edit the release workflow.

```bash
vi .gitea/workflows/release.yml
```

Replace the TODO sections with:

```yaml
- name: Build image
  run: |
    docker build \
      -t "$REGISTRY/$IMAGE:${{ steps.version.outputs.VERSION }}" \
      .

- name: Push image to Gitea registry
  run: |
    docker push "$REGISTRY/$IMAGE:${{ steps.version.outputs.VERSION }}"
```

## Step 3

Commit the workflow.

```bash
git add .gitea/workflows/release.yml

git commit -m "Publish Docker image during release"
```

## Step 4

Push to the main branch.

```bash
git push origin main
```

<img width="722" height="296" alt="push" src="https://github.com/user-attachments/assets/fca75580-ee96-488a-9dd3-375a573fcc0d" />

<img width="962" height="577" alt="jobs" src="https://github.com/user-attachments/assets/3cf8fd5c-692c-4471-918f-cd235a9093eb" />

## Step 5

Open Gitea.

```text
http://localhost:3000
```

Login:

```text
Username: gitea-admin

Password: gitea2026
```

## Step 6

Create the release.

Navigate:

```text
Repository
↓

Releases
↓

New Release
```

Fill in:

```text
Tag:
v0.1.0

Target:
main

Title:
Fraud Detector v0.1.0
```

<img width="1080" height="546" alt="release tag" src="https://github.com/user-attachments/assets/613e0c52-fb76-4316-90a8-11a0e89242fb" />

Click:

```text
Publish Release
```

Publishing the release creates the `v0.1.0` tag, which automatically triggers the workflow.

## Step 7

Wait for the workflow to complete.

Expected:

```text
✔ Build and Publish
```

<img width="1172" height="762" alt="release job" src="https://github.com/user-attachments/assets/63667fea-1296-43ad-bdf9-eb532e717e96" />

## Step 8

Verify the release.

Open:

```text
Releases

↓

v0.1.0
```

Expected asset:

```text
metrics.json
```

<img width="1682" height="567" alt="tag" src="https://github.com/user-attachments/assets/f13cc3b1-b571-4aaf-86e5-ca0a65353298" />

# Workflow

```text
Create Release (v0.1.0)
          │
          ▼
Git Tag Created
          │
          ▼
Release Workflow Triggered
          │
          ▼
Docker Build
          │
          ▼
Docker Push
          │
          ▼
Train Model
          │
          ▼
Generate metrics.json
          │
          ▼
Attach metrics.json
          │
          ▼
Gitea Release + Container Package
```

# Fun Message 😄

*"A release isn't just a Git tag—it's the complete package: versioned code, a deployable container, and the evidence that proves how well the model performs. 🚀📦"*
