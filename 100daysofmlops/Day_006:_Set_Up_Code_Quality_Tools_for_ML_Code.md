## Instruction

The xFusionCorp Industries ML team enforces strict code quality checks using:

* `ruff` for linting
* `black` for code formatting

The project located at:

```bash id="n7x2p5"
/root/code/fraud-detection/
```

was failing both tools.

The task was to inspect the configuration and source files, fix all issues, and ensure both commands exit successfully.



# Requirements

The corrected project needed to satisfy:

* `ruff` and `black` must use:

```toml id="v3m8q1"
line-length = 120
```

* Ruff lint rule selection must include:

```toml id="k5r2x9"
E, F, W, I
```

* Ruff configuration must use the new schema:

```toml id="d8p4m6"
[tool.ruff.lint]
```

* The following commands must pass successfully:

```bash id="u2x9n4"
ruff check src/
```

```bash id="f6v1m8"
black --check src/
```



# Existing pyproject.toml

```toml id="p9x4m2"
[project]
name = "fraud-detection"
version = "0.1.0"

[tool.ruff]
line-length = 88
select = ["E", "F"]

[tool.black]
line-length = 100
```



# Problems Identified 🚨

## 1. Incorrect Line Lengths

* Ruff used `88`
* Black used `100`

Required value:

```toml id="q7m2v5"
120
```



## 2. Deprecated Ruff Configuration

The configuration used:

```toml id="t1x8r3"
select = ["E", "F"]
```

directly under `[tool.ruff]`.

Newer versions of Ruff require:

```toml id="h5n4p7"
[tool.ruff.lint]
```



## 3. Missing Lint Rules

Required rule groups:

| Rule | Purpose              |
| ---- | -------------------- |
| E    | pycodestyle errors   |
| F    | pyflakes             |
| W    | pycodestyle warnings |
| I    | import sorting       |



## 4. Source Code Formatting Issues

Files inside `src/` likely contained:

* Improper formatting
* Import ordering issues
* Extra whitespace
* Long lines



# Solution

## ✅ Corrected pyproject.toml

```toml id="c8v3m1"
[project]
name = "fraud-detection"
version = "0.1.0"

[tool.ruff]
line-length = 120

[tool.ruff.lint]
select = ["E", "F", "W", "I"]

[tool.black]
line-length = 120
```

<img width="741" height="426" alt="conf" src="https://github.com/user-attachments/assets/a43c2f48-cbec-4dae-819a-56abadf2d533" />


# Step-by-Step Fix

## 1. Navigate to the Project Directory

```bash id="r4m7x2"
cd /root/code/fraud-detection
```



## 2. Edit pyproject.toml

```bash id="j8p2v5"
vi pyproject.toml
```

Update it with the corrected configuration.



## 3. Automatically Fix Ruff Issues

Run:

```bash id="f1x9m6"
ruff check src/ --fix
```

This automatically resolves:

* Import ordering
* Unused imports
* Simple formatting issues



## 4. Format Code Using Black

Run:

```bash id="y6n3q8"
black src/
```

This reformats source files according to Black standards.



## 5. Verify Ruff Passes

```bash id="k2v7m4"
ruff check src/
```

Expected result:

```bash id="u5x1r9"
All checks passed!
```

<img width="677" height="212" alt="ruff" src="https://github.com/user-attachments/assets/f6e13c31-b666-4f2c-a480-b7585519b8f1" />


## 6. Verify Black Passes

```bash id="w8m4p2"
black --check src/
```

Expected result:

```bash id="c3n7x5"
All done! ✨ 🍰 ✨
```

<img width="700" height="136" alt="black" src="https://github.com/user-attachments/assets/0fd4e74b-bfc5-4916-a122-b238cd0110f1" />


# Understanding the Concept 💡

## What is Ruff?

Ruff is a fast Python linter that checks for:

* Syntax issues
* Style problems
* Import sorting
* Unused variables
* Code quality violations



## What is Black?

Black is an opinionated Python code formatter.

It automatically formats code into a consistent style.



## Why Use Both?

| Tool  | Purpose                   |
| ----- | ------------------------- |
| Ruff  | Finds code issues         |
| Black | Formats code consistently |

Together they improve:

* Readability
* Team collaboration
* CI/CD quality checks
* Production readiness



# Challenges Faced 🚧

* Deprecated Ruff configuration schema
* Import sorting failures
* Line length mismatches
* Formatting inconsistencies in source files



# Key Learnings 📚

* Learned modern Ruff configuration standards
* Understood automated lint fixing
* Practiced Python formatting workflows
* Explored CI-friendly code quality tooling
* Learned the difference between linting and formatting



# Commands Used 🖥️

```bash id="a9m2v6"
cd /root/code/fraud-detection

vi pyproject.toml

ruff check src/ --fix

black src/

ruff check src/

black --check src/
```



# Fun Message 😄

*"Black reformats your code without asking, Ruff judges it instantly… together they’re basically Python’s strict parents 😄"*

