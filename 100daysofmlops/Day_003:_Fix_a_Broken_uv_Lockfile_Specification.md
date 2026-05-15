## Instruction

The xFusionCorp Industries ML team uses **uv** and lockfiles to maintain reproducible Python environments across different systems.

A teammate left behind a `requirements.in` file that did not follow the team’s standards. The task was to inspect the file, correct the dependency specifications, and generate a fully pinned lockfile.

### Requirements

The corrected `requirements.in` file had to:

* Include exactly these four top-level packages:

  * `scikit-learn`
  * `mlflow`
  * `pandas`
  * `numpy`
* Ensure every package had a valid version constraint compatible with PyPI

The specification file was located at:

```bash id="h2k7m1"
/root/code/fraud-detection/requirements.in
```

After correcting the file, compile it into a lockfile using:

```bash id="w9p3x4"
uv pip compile requirements.in -o requirements.txt
```

The generated `requirements.txt` needed to:

* Pin all top-level packages using `==`
* Include all resolved transitive dependencies



# Solution

## ✅ Steps to Complete the Task

### 1. Navigate to the Project Directory

```bash id="q5n8r2"
cd /root/code/fraud-detection
```



### 2. Inspect the Existing requirements.in File

View the contents:

```bash id="t6v1p9"
cat requirements.in
```

Open the file for editing:

```bash id="y4m8k3"
vi requirements.in
```

or

```bash id="c2x7f5"
nano requirements.in
```



### 3. Correct the Dependency Specifications

Updated `requirements.in`:

```txt id="a8d4n7"
numpy>=1.26.0
pandas>=2.2.0
scikit-learn>=1.5.0
mlflow>=2.12.0
```

<img width="751" height="297" alt="reqire" src="https://github.com/user-attachments/assets/6c4a1393-0b12-4637-8f14-c5030c689c6c" />


This ensures:

* Only the required four packages are listed
* All version constraints are valid and resolvable by `uv`



### 4. Compile the Lockfile Using UV

Run:

```bash id="m7q2z8"
uv pip compile requirements.in -o requirements.txt
```

This command:

* Resolves dependencies from PyPI
* Pins exact package versions
* Generates all transitive dependencies automatically



### 5. Verify the Generated requirements.txt

Check the contents:

```bash id="r1v6k9"
cat requirements.txt
```

Example output:

```txt id="p3x8f1"
numpy==2.x.x
pandas==2.x.x
scikit-learn==1.x.x
mlflow==2.x.x
```

Along with additional dependencies resolved by `uv`.



# Understanding the Concept 💡

## What is `requirements.in`?

A high-level dependency specification file where developers define only direct dependencies.

Example:

```txt id="z5n3k7"
pandas>=2.2.0
```



## What is `requirements.txt`?

A fully pinned dependency lockfile containing:

* Exact package versions
* Transitive dependencies

Example:

```txt id="u2m9d4"
pandas==2.2.3
numpy==2.1.1
```

This guarantees consistent environments across systems.



## Why Use UV?

`uv` is a modern Python package manager focused on:

* Speed ⚡
* Dependency resolution
* Reproducible environments
* Lockfile generation

It works similarly to `pip-tools` but much faster.



# Challenges Faced 🚧

* Invalid or unsupported package versions
* Extra packages listed in `requirements.in`
* Forgetting to pin dependencies through lockfile generation
* Understanding the difference between direct and transitive dependencies



# Key Learnings 📚

* Learned how dependency locking works
* Understood the role of reproducible environments in MLOps
* Explored `uv pip compile`
* Learned the difference between:

  * `requirements.in`
  * `requirements.txt`
* Practiced dependency management best practices



# Commands Used 🖥️

```bash id="f8x3m6"
cd /root/code/fraud-detection

cat requirements.in

vi requirements.in

uv pip compile requirements.in -o requirements.txt

cat requirements.txt
```

<img width="747" height="187" alt="cmd" src="https://github.com/user-attachments/assets/b1b407ce-ea02-4ba5-b4d9-c69a891ff95e" />


# Fun Message 😄

*"Dependency management is basically telling Python: ‘Please break on someone else’s machine, not mine.’ 😄"*

