## Instruction

A teammate added the dataset:

```bash id="m7x3p1"
data/raw/transactions.csv
```

directly into Git inside the xFusionCorp Industries `fraud-detection` repository.

However, the team standard requires:

> Every dataset under `data/` must be tracked using DVC instead of Git.

The task was to:

* Stop Git from tracking the dataset
* Keep the file safely on disk
* Track it using DVC
* Generate the `.dvc` pointer file
* Update `.gitignore`
* Commit the changes into Git



# Requirements

The repository already had:

* Git initialized ✅
* DVC initialized ✅

The dataset:

```bash id="v4m8x2"
data/raw/transactions.csv
```

needed to be moved from Git tracking → DVC tracking.

Final requirements:

* Dataset must remain on disk
* Git should stop tracking the CSV directly
* DVC should create a `.dvc` pointer file
* `data/raw/.gitignore` must exclude the dataset
* Commit message must be:

```bash id="q2m7x5"
Track transactions dataset with DVC
```



# Solution

## ✅ Step 1 - Navigate to the Repository

```bash id="p8x3m1"
cd /root/code/fraud-detection
```



# Step 2 - Stop Git Tracking the Dataset

Remove the file from Git tracking **without deleting it locally**:

```bash id="k5m9x4"
git rm --cached data/raw/transactions.csv
```

Explanation:

| Flag               | Purpose                     |
| ------------------ | --------------------------- |
| --cached           | Removes from Git index only |
| File stays on disk | ✅                           |

<img width="982" height="83" alt="rmgit" src="https://github.com/user-attachments/assets/2c569cb0-2ac6-4449-a292-33f44e9206c8" />


# Step 3 - Track the Dataset with DVC

Run:

```bash id="r7x2m6"
dvc add data/raw/transactions.csv
```

This generates:

```bash id="t1m8x5"
data/raw/transactions.csv.dvc
```

and updates:

```bash id="f4x9m2"
data/raw/.gitignore
```

to exclude the actual dataset file.

<img width="971" height="252" alt="dvc add" src="https://github.com/user-attachments/assets/cf8aad2e-6b39-497f-8ebc-8e8405f8d93d" />


# Step 4 - Verify Generated Files

Check the directory:

```bash id="u6m3x8"
ls data/raw/
```

Expected output:

```bash id="y2x7m4"
transactions.csv
transactions.csv.dvc
.gitignore
```



# Step 5 - Stage DVC Tracking Files

Add the new metadata files:

```bash id="j8x4m2"
git add data/raw/transactions.csv.dvc data/raw/.gitignore
```

Also stage the Git removal:

```bash id="c3m7x5"
git add -u
```

<img width="990" height="73" alt="git add" src="https://github.com/user-attachments/assets/79bbb2ca-5790-458a-9e44-9726161af2cc" />


# Step 6 - Commit the Changes

Create the Git commit:

```bash id="a9m3x6"
git commit -m "Track transactions dataset with DVC"
```

Expected output:

```bash id="z4x8m1"
[main abc123] Track transactions dataset with DVC
```

<img width="993" height="172" alt="git comm" src="https://github.com/user-attachments/assets/d517c286-3c4f-4eac-a2e0-8a883179617d" />


# Step 7 - Verify Repository Status

```bash id="n5m2x7"
git status
```

Expected output:

```bash id="w8x4m3"
nothing to commit, working tree clean
```



# Understanding the Concept 💡

## Why Not Store Large Datasets in Git?

Git is optimized for:

* Source code
* Small text files

Large datasets cause:

* Huge repository sizes
* Slow cloning
* Poor performance



## How DVC Solves This

DVC stores:

| File Type        | Location          |
| ---------------- | ----------------- |
| Dataset metadata | Git               |
| Actual dataset   | DVC cache/storage |

Git tracks only lightweight `.dvc` files.



## What is a `.dvc` File?

Example:

```yaml id="x3m7p2"
outs:
- md5: abc123
  path: transactions.csv
```

It acts like a pointer to the actual dataset.



## Why Update `.gitignore`?

To prevent Git from accidentally re-tracking large datasets.



# Challenges Faced 🚧

* Understanding Git vs DVC responsibilities
* Preventing accidental dataset deletion
* Managing `.gitignore` automatically
* Learning DVC pointer workflows



# Key Learnings 📚

* Learned how DVC tracks datasets
* Practiced moving files from Git → DVC
* Understood `.dvc` pointer files
* Explored reproducible dataset versioning
* Learned scalable ML data management practices



# Commands Used 🖥️

```bash id="b6x1m9"
cd /root/code/fraud-detection

git rm --cached data/raw/transactions.csv

dvc add data/raw/transactions.csv

git add data/raw/transactions.csv.dvc data/raw/.gitignore

git add -u

git commit -m "Track transactions dataset with DVC"

git status
```



# Fun Message 😄

*"Git stores your code, DVC stores your giant CSVs… because nobody wants a 5 GB Git clone 😄"*


