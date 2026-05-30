## Instruction

The xFusionCorp Industries ML team uses **Git + DVC** together to manage different versions of datasets and models.

The goal was to:

* Preserve the current dataset version as **v1.0**
* Create a new branch using an improved dataset
* Retrain the pipeline
* Commit the new version
* Verify that switching branches restores the correct dataset version automatically



# Project Setup

Existing tracked dataset:

```bash id="d1"
data/raw/transactions.csv
```

Improved dataset provided:

```bash id="d2"
data/raw/transactions_v2.csv
```

The improved dataset must remain in the repository and should not be deleted.



# Requirements

1. Tag current state as:

```bash id="d3"
v1.0
```

2. Create branch:

```bash id="d4"
v2-improved
```

3. Replace dataset with the improved version

4. Re-track dataset using DVC

5. Re-run the pipeline

6. Commit changes

7. Switch back to `main`

8. Restore original dataset using:

```bash id="d5"
dvc checkout
```



# Solution

## ✅ Step 1 - Navigate to the Repository

```bash id="d6"
cd /root/code/fraud-detection
```



# Step 2 - Verify Current Status

Check repository status:

```bash id="d7"
git status

dvc status
```

Ensure everything is clean before tagging.



# Step 3 - Tag the Current Version

Create the v1 release tag:

```bash id="d8"
git tag v1.0
```

Verify:

```bash id="d9"
git tag
```

Expected:

```text id="d10"
v1.0
```



# Step 4 - Create the Improved Branch

```bash id="d11"
git checkout -b v2-improved
```

Verify:

```bash id="d12"
git branch
```

Expected:

```text id="d13"
* v2-improved
```



# Step 5 - Replace the Dataset

Copy the improved dataset over the tracked one:

```bash id="d14"
cp data/raw/transactions_v2.csv data/raw/transactions.csv
```

The original file is replaced, but:

```bash id="d15"
transactions_v2.csv
```

remains untouched.



# Step 6 - Update DVC Tracking

Tell DVC that the dataset contents changed:

```bash id="d16"
dvc add data/raw/transactions.csv
```

This updates:

```bash id="d17"
data/raw/transactions.csv.dvc
```

with the new dataset hash.

<img width="982" height="328" alt="dvc add" src="https://github.com/user-attachments/assets/8025fe5d-01d5-4df7-a319-8c4a1afe7a3b" />


# Step 7 - Re-run the Pipeline

Execute:

```bash id="d18"
dvc repro
```

Expected execution:

```text id="d19"
process_data
      ↓
split_data
      ↓
train
```

New artifacts generated:

```bash id="d20"
data/processed/*
models/model.pkl
metrics.json
```

<img width="797" height="423" alt="dvc repo" src="https://github.com/user-attachments/assets/0231f72d-3c8f-44cf-901f-57c285d28342" />


# Step 8 - Commit the New Dataset Version

Stage changes:

```bash id="d21"
git add .
```

Commit:

```bash id="d22"
git commit -m "Add improved dataset version"
```



# Step 9 - Verify New Dataset Hash

Check:

```bash id="d23"
cat data/raw/transactions.csv.dvc
```

You should see a different MD5 hash than the one recorded under v1.0.

<img width="958" height="385" alt="git add" src="https://github.com/user-attachments/assets/040b5f83-9e71-4dc0-98d1-3eb8dfeaa7df" />


# Restoring the Original Dataset

## Step 10 - Switch Back to Main

```bash id="d24"
git checkout main
```

At this point Git restores:

```bash id="d25"
transactions.csv.dvc
```

to the version recorded on main.

However the dataset file itself may still contain v2 content.



## Step 11 - Restore the Dataset Using DVC

Run:

```bash id="d26"
dvc checkout
```

DVC reads:

```bash id="d27"
transactions.csv.dvc
```

and restores the exact dataset version tracked by the current branch.



## Step 12 - Verify the Restoration

Check:

```bash id="d28"
dvc status
```

Expected:

```text id="d29"
Data and pipelines are up to date.
```

The dataset now matches the hash recorded by:

```bash id="d30"
git tag v1.0
```



# Understanding the Concept 💡

## Why Combine Git and DVC?

Git tracks:

* Code
* Configurations
* `.dvc` files

DVC tracks:

* Datasets
* Models
* Pipeline outputs

Together they allow complete project versioning.



## What Happens When Branches Change?

Git switches:

```bash id="d31"
transactions.csv.dvc
```

DVC then restores:

```bash id="d32"
transactions.csv
```

to the correct version for that branch.

<img width="970" height="158" alt="gitstatus" src="https://github.com/user-attachments/assets/f762d3c7-27dd-46f2-bcf5-292691c3c4d0" />


## Why Use Tags?

Tags mark important milestones:

```bash id="d33"
v1.0
```

acts as a permanent reference point.

Future rollback becomes easy:

```bash id="d34"
git checkout v1.0
dvc checkout
```



# Challenges Faced 🚧

* Understanding Git vs DVC responsibilities
* Managing dataset version transitions
* Remembering to run `dvc checkout`
* Tracking dataset hashes across branches



# Key Learnings 📚

* Learned how Git and DVC work together
* Practiced dataset version management
* Used tags to create reproducible releases
* Explored branch-based ML workflows
* Learned how DVC restores historical datasets



# Commands Used 🖥️

```bash id="d35"
cd /root/code/fraud-detection

git tag v1.0

git checkout -b v2-improved

cp data/raw/transactions_v2.csv data/raw/transactions.csv

dvc add data/raw/transactions.csv

dvc repro

git add .

git commit -m "Add improved dataset version"

git checkout main

dvc checkout

dvc status
```



# Fun Message 😄

*"With Git alone you version code. With Git + DVC you version reality 😄📦"*
