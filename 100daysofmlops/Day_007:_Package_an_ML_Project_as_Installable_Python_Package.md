## Instruction

The xFusionCorp Industries deployment team needed the `fraud-detection` project packaged as an installable Python distribution.

A draft `pyproject.toml` already existed inside:

```bash id="m8x3p1"
/root/code/fraud-detection/
```

However, the package configuration did not meet the organization’s packaging standards.

The task was to:

* Fix the `pyproject.toml`
* Configure proper package metadata
* Build the project into a Python wheel



# Requirements

The corrected `pyproject.toml` had to satisfy all of the following:

| Requirement        | Value                             |
| ------------------ | --------------------------------- |
| Build Backend      | `setuptools.build_meta`           |
| Build Requirements | `setuptools>=61.0`, `wheel`       |
| Package Name       | `fraud_detection`                 |
| Version            | `0.1.0`                           |
| Python Version     | `>=3.10`                          |
| Dependencies       | `scikit-learn`, `pandas`, `numpy` |

The source code already existed under:

```bash id="p2v7n4"
src/fraud_detection/
```

No Python source files needed modification.



# Existing Incorrect pyproject.toml

```toml id="q5m1x8"
[project]
name = "fraud-detection"
version = "0.0.1"
description = "Fraud detection model for xFusionCorp Industries"
requires-python = ">=3.8"
dependencies = []

[tool.setuptools.packages.find]
where = ["src"]
```


# Solution

## ✅ Corrected pyproject.toml

```toml id="k3x7m1"
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "fraud_detection"
version = "0.1.0"
description = "Fraud detection model for xFusionCorp Industries"
requires-python = ">=3.10"
dependencies = [
    "scikit-learn",
    "pandas",
    "numpy"
]

[tool.setuptools.packages.find]
where = ["src"]
```

<img width="696" height="527" alt="pom" src="https://github.com/user-attachments/assets/246bdd26-c951-4435-bfba-8a15b5ef1100" />


# Step-by-Step Execution

## 1. Navigate to the Project Directory

```bash id="w2m8p5"
cd /root/code/fraud-detection
```



## 2. Edit pyproject.toml

```bash id="h9x4n1"
vi pyproject.toml
```

Replace the contents with the corrected configuration.



## 3. Build the Package

Run:

```bash id="q6v2m7"
python3 -m build
```

<img width="652" height="202" alt="res" src="https://github.com/user-attachments/assets/5a2e9126-fbc0-49f3-921f-0fcb4bd46d97" />


This command creates:

* Source distribution (`.tar.gz`)
* Wheel package (`.whl`)



## 4. Verify the Build Output

Check the `dist/` directory:

```bash id="z4p8x2"
ls dist/
```

Expected output:

```bash id="j1m7q5"
fraud_detection-0.1.0-py3-none-any.whl
fraud_detection-0.1.0.tar.gz
```

<img width="291" height="225" alt="dist" src="https://github.com/user-attachments/assets/d7c6daa3-e26a-4e61-9f4c-04a02feb395a" />


# Understanding the Concept 💡

## What is a Python Wheel?

A `.whl` file is a built Python package format that allows fast installation without rebuilding source code.

Example:

```bash id="c5n9x4"
pip install fraud_detection-0.1.0-py3-none-any.whl
```



## Why Use pyproject.toml?

`pyproject.toml` is the modern Python packaging standard replacing older `setup.py` workflows.

It defines:

* Package metadata
* Dependencies
* Build configuration
* Tool settings



## What is setuptools.build_meta?

It is the official setuptools backend used for building modern Python packages.



# Challenges Faced 🚧

* Incorrect distribution naming
* Missing build-system configuration
* Dependency declaration issues
* Understanding wheel generation



# Key Learnings 📚

* Learned modern Python packaging standards
* Understood wheel generation workflows
* Explored `pyproject.toml`
* Learned how Python distributions are built
* Practiced dependency and metadata configuration



# Commands Used 🖥️

```bash id="a8x3m6"
cd /root/code/fraud-detection

vi pyproject.toml

python3 -m build

ls dist/
```



# Fun Message 😄

*"Packaging Python projects is basically wrapping your code like a gift so pip can open it anywhere 🎁🐍"*


