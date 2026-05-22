## Instruction

The xFusionCorp Industries ML team started adopting **DVC (Data Version Control)** to manage datasets and model files separately from source code.

The task was to initialize DVC inside the existing Git repository located at:

```bash id="m7x3p1"
/root/code/fraud-detection/
```

and record the initialization inside Git.



# Requirements

The task required:

* Initializing DVC inside the existing Git repository
* Creating:

  * `.dvc/`
  * `.dvcignore`
* Staging all generated DVC files
* Creating a Git commit with the message:

```bash id="v4m8x2"
Initialize DVC
```

After successful initialization:

* The DVC extension detects the repository
* A **DVC TRACKED** section appears in VS Code
* A DVC indicator appears in the status bar



# Solution

## ✅ Step 1 - Navigate to the Repository

```bash id="p2x7m5"
cd /root/code/fraud-detection
```



# Step 2 - Initialize DVC

Run:

```bash id="n8v3m1"
dvc init
```

Expected output:

```bash id="q5m9x4"
Initialized DVC repository.
```

This creates:

```bash id="r7x2m6"
.dvc/
.dvcignore
```

<img width="992" height="397" alt="init" src="https://github.com/user-attachments/assets/9f0127dd-4f7c-47e1-94b8-74bf08859143" />


# Step 3 - Verify Generated Files

List hidden files:

```bash id="t1m8x5"
ls -la
```

Expected output includes:

```bash id="f4x9m2"
.dvc/
.dvcignore
```

<img width="472" height="262" alt="la" src="https://github.com/user-attachments/assets/c6ecf65c-db65-4c7f-ab02-86c6bcbd9be0" />


# Step 4 - Stage DVC Files in Git

Add all generated files:

```bash id="u6m3x8"
git add .dvc .dvcignore
```



# Step 5 - Commit the Initialization

Create the Git commit:

```bash id="y2x7m4"
git commit -m "Initialize DVC"
```

Expected output:

```bash id="k5m1x9"
[main abc123] Initialize DVC
```



# Step 6 - Verify Git Status

```bash id="j8x4m2"
git status
```

Expected output:

```bash id="c3m7x5"
nothing to commit, working tree clean
```

<img width="710" height="218" alt="git" src="https://github.com/user-attachments/assets/ca8a8d4e-889a-429d-9cc4-8b728254ca0f" />


# Understanding the Concept 💡

## What is DVC?

DVC (Data Version Control) helps manage:

* Large datasets
* Machine learning models
* Experiment tracking

without storing huge files directly inside Git.



## Why Use DVC in MLOps?

Git works great for source code but struggles with:

* Large datasets
* Model artifacts
* Frequent binary file changes

DVC solves this by:

* Tracking metadata in Git
* Storing actual data separately



## What Does `dvc init` Do?

It creates:

| File/Folder  | Purpose                    |
| ------------ | -------------------------- |
| `.dvc/`      | DVC internal configuration |
| `.dvcignore` | Ignore rules for DVC       |

and links DVC with the existing Git repository.



# Challenges Faced 🚧

* Understanding the difference between Git and DVC
* Forgetting to commit DVC metadata
* Missing hidden files during verification



# Key Learnings 📚

* Learned how DVC integrates with Git
* Understood data versioning workflows
* Practiced initializing DVC repositories
* Explored ML data management concepts
* Learned how MLOps teams separate code and datasets



# Commands Used 🖥️

```bash id="a9m3x6"
cd /root/code/fraud-detection

dvc init

ls -la

git add .dvc .dvcignore

git commit -m "Initialize DVC"

git status
```



# Fun Message 😄

*"Git handles your code, DVC handles your giant datasets… together they’re basically the Avengers of MLOps 😄"*

