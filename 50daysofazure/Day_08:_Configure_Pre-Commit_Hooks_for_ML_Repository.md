## Instruction

The xFusionCorp Industries ML team enforces automated code quality checks using `pre-commit`.

A draft configuration file already existed inside the Git repository at:

```bash id="m7x3p2"
/root/code/fraud-detection/
```

However, running:

```bash id="q4v8n1"
pre-commit run --all-files
```

failed because the configuration did not match the team’s standards.

The task was to:

* Inspect and fix `.pre-commit-config.yaml`
* Configure all required hooks correctly
* Install the hooks
* Run them successfully against all tracked files



# Requirements

The corrected configuration had to include these hooks:

| Hook                | Repository                  |
| ------------------- | --------------------------- |
| trailing-whitespace | pre-commit/pre-commit-hooks |
| end-of-file-fixer   | pre-commit/pre-commit-hooks |
| check-yaml          | pre-commit/pre-commit-hooks |
| ruff                | astral-sh/ruff-pre-commit   |
| black               | psf/black-pre-commit-mirror |

Additional requirements:

* Every repository entry must include a `rev:` field
* Hooks must use valid IDs
* All hooks must run successfully



# Existing Incorrect Configuration

```yaml id="v2m8x5"
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v2.3.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check_yaml

  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.1.0
    hooks:
      - id: ruff-lint

  - repo: https://github.com/psf/black-pre-commit-mirror
    hooks:
      - id: black
```



# Solution

## ✅ Corrected .pre-commit-config.yaml

```yaml id="c8m3x1"
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.6.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.5.0
    hooks:
      - id: ruff

  - repo: https://github.com/psf/black-pre-commit-mirror
    rev: 24.4.2
    hooks:
      - id: black
```

<img width="1020" height="495" alt="yaml" src="https://github.com/user-attachments/assets/a0549f84-f79c-49e9-b6aa-b9657a48c4f8" />


# Step-by-Step Execution

## 1. Navigate to the Repository

```bash id="j7x2m5"
cd /root/code/fraud-detection
```



## 2. Edit the Configuration File

```bash id="t4v8n1"
vi .pre-commit-config.yaml
```

Replace the contents with the corrected configuration.



## 3. Install Pre-Commit Hooks

Register the hooks with Git:

```bash id="w9m3x6"
pre-commit install
```

Expected output:

```bash id="q2v7p4"
pre-commit installed at .git/hooks/pre-commit
```



## 4. Run Hooks Against All Files

```bash id="r5n1x8"
pre-commit run --all-files
```

Expected result:

```bash id="u8m4v2"
trailing-whitespace................Passed
end-of-file-fixer.................Passed
check-yaml........................Passed
ruff..............................Passed
black.............................Passed
```

<img width="737" height="362" alt="run" src="https://github.com/user-attachments/assets/0355a165-3ae3-4472-8b08-3b92ed0afab3" />


# Understanding the Concept 💡

## What is Pre-Commit?

`pre-commit` automatically runs checks before code is committed to Git.

It helps teams enforce:

* Formatting standards
* Linting rules
* YAML validation
* Import ordering
* General code quality



## Why Use Hooks?

Hooks automate repetitive quality checks so developers don’t forget them.

Instead of manually running:

```bash id="p1x6m9"
ruff
black
```

Git automatically validates code before every commit.



## Why Pin Versions with rev?

Pinning versions ensures:

* Reproducibility
* Stable CI/CD pipelines
* Consistent behavior across developer machines



# Challenges Faced 🚧

* Incorrect hook names
* Deprecated Ruff repository
* Missing version pins
* Understanding YAML hook configuration syntax



# Key Learnings 📚

* Learned how Git hooks automate quality checks
* Explored modern Ruff pre-commit integration
* Understood YAML-based tool configuration
* Practiced configuring pre-commit workflows
* Learned the importance of reproducible tooling



# Commands Used 🖥️

```bash id="a6x3m8"
cd /root/code/fraud-detection

vi .pre-commit-config.yaml

pre-commit install

pre-commit run --all-files
```



# Fun Message 😄

*"Pre-commit is basically that strict teammate who checks your work before you’re allowed to push 😄"*


