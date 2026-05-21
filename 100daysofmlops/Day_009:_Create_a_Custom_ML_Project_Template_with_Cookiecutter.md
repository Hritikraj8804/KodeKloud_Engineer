## Instruction

The xFusionCorp Industries ML platform team maintains a reusable `Cookiecutter` template for generating standardized ML project structures.

A draft template already existed at:

```bash id="m7x3p5"
/root/code/mlops-template/
```

However, the template was broken and failed to render correctly.

The task was to:

* Inspect and fix the Cookiecutter template
* Correct variable definitions
* Fix template rendering logic
* Generate a working ML project from the template



# Requirements

The corrected `cookiecutter.json` needed to declare:

| Variable       | Default                      |
| -------------- | ---------------------------- |
| project_name   | my-ml-project                |
| author         | xFusionCorp                  |
| python_version | 3.11                         |
| ml_framework   | sklearn, pytorch, tensorflow |



# requirements.txt Logic

The generated `requirements.txt` had to contain:

| Framework  | Dependency   |
| ---------- | ------------ |
| sklearn    | scikit-learn |
| pytorch    | torch        |
| tensorflow | tensorflow   |



# README.md Requirements

The generated `README.md` needed to reference:

* `project_name`
* `author`

using Cookiecutter variables.



# Required Template Structure

```bash id="q2v8m1"
{{cookiecutter.project_name}}/
├── README.md
├── requirements.txt
├── data/
├── models/
├── src/
└── tests/
```



# Solution

## ✅ Step 1 - Navigate to the Template Directory

```bash id="p5m2x7"
cd /root/code/mlops-template
```



# Step 2 - Fix cookiecutter.json

## Corrected cookiecutter.json

```json id="n8x4m2"
{
  "project_name": "my-ml-project",
  "author": "xFusionCorp",
  "python_version": "3.11",
  "ml_framework": [
    "sklearn",
    "pytorch",
    "tensorflow"
  ]
}
```

<img width="815" height="307" alt="json" src="https://github.com/user-attachments/assets/f4ddb1ab-a13d-4b47-89c6-14076e32600c" />


# Step 3 - Fix Template Directory Structure

Correct template structure:

```bash id="k4m9x1"
mlops-template/
├── cookiecutter.json
└── {{cookiecutter.project_name}}/
    ├── README.md
    ├── requirements.txt
    ├── data/
    ├── models/
    ├── src/
    └── tests/
```

Create directories if missing:

```bash id="r7v2m5"
mkdir -p "{{cookiecutter.project_name}}"/{data,models,src,tests}
```



# Step 4 - Fix README.md Template

## Corrected README.md

```md id="u2m8x6"
# {{ cookiecutter.project_name }}

ML project created by {{ cookiecutter.author }}.

Python Version: {{ cookiecutter.python_version }}
```

<img width="876" height="282" alt="redme" src="https://github.com/user-attachments/assets/d7f681f5-4d02-439b-a04f-089a82016c9b" />


This ensures both required variables render properly.



# Step 5 - Fix requirements.txt Template

## Corrected requirements.txt

```jinja id="w5x1m9"
{% if cookiecutter.ml_framework == "sklearn" %}
scikit-learn
{% elif cookiecutter.ml_framework == "pytorch" %}
torch
{% elif cookiecutter.ml_framework == "tensorflow" %}
tensorflow
{% endif %}
```

<img width="882" height="282" alt="require" src="https://github.com/user-attachments/assets/a4e6f380-6d42-4cc9-926f-b75934e8b337" />


This dynamically installs dependencies based on the selected ML framework.



# Step 6 - Generate the Project

Run:

```bash id="j8m3x2"
cookiecutter /root/code/mlops-template/ -o /root/code/ --no-input project_name=churn-model ml_framework=sklearn
```

This generates:

```bash id="q6v1m4"
/root/code/churn-model/
```

<img width="935" height="71" alt="cmd" src="https://github.com/user-attachments/assets/cb7906d4-febf-4e96-b95a-076f7214e002" />


# Step 7 - Verify the Generated Project

Check the structure:

```bash id="f9x2m7"
tree /root/code/churn-model
```

Expected structure:

```bash id="t4m8x1"
churn-model/
├── README.md
├── requirements.txt
├── data/
├── models/
├── src/
└── tests/
```



# Step 8 - Verify requirements.txt

```bash id="y2m5x8"
cat /root/code/churn-model/requirements.txt
```

Expected output:

```txt id="n7v3m1"
scikit-learn
```



# Step 9 - Verify README.md

```bash id="p4x8m2"
cat /root/code/churn-model/README.md
```

Expected output should mention:

```txt id="h6m1x9"
xFusionCorp
```

<img width="552" height="222" alt="res" src="https://github.com/user-attachments/assets/4f57d920-dd20-4d7a-b82c-46be694d3c99" />


# Understanding the Concept 💡

## What is Cookiecutter?

Cookiecutter is a project templating tool that generates projects from reusable templates.

It helps teams standardize:

* Project structures
* Configurations
* Dependencies
* Documentation



## Why Use Templates in MLOps?

Templates ensure:

* Consistency across projects
* Faster onboarding
* Reduced setup errors
* Reusable engineering standards



## What is Jinja Templating?

Cookiecutter uses Jinja syntax:

```jinja id="c5m2x7"
{{ variable }}
```

and conditional logic:

```jinja id="v8x4m1"
{% if condition %}
```

to dynamically generate files.



# Challenges Faced 🚧

* Incorrect variable definitions
* Broken Jinja templating logic
* Missing directories
* Improper framework dependency handling
* Template rendering failures



# Key Learnings 📚

* Learned how Cookiecutter templates work
* Practiced Jinja templating syntax
* Understood dynamic dependency generation
* Explored project scaffolding workflows
* Learned reusable ML project standardization



# Commands Used 🖥️

```bash id="a9m3x5"
cd /root/code/mlops-template

vi cookiecutter.json

mkdir -p "{{cookiecutter.project_name}}"/{data,models,src,tests}

vi "{{cookiecutter.project_name}}/README.md"

vi "{{cookiecutter.project_name}}/requirements.txt"

cookiecutter /root/code/mlops-template/ -o /root/code/ --no-input project_name=churn-model ml_framework=sklearn

tree /root/code/churn-model
```



# Fun Message 😄

*"Cookiecutter is basically copy-paste on steroids… but smarter 😄🍪"*


