## Instruction

The xFusionCorp Industries ML team uses a Makefile to orchestrate common tasks—data processing, training, testing, and cleanup. A draft Makefile exists at /root/code/fraud-detection/Makefile, but make all does not complete successfully. Bring the Makefile in line with the team's standard. 

Change into /root/code/fraud-detection/ and run make all to observe the current failure. 

The corrected Makefile must declare the following six targets and behaviour: 

setup – Creates a virtual environment at mlops-venv/ and installs dependencies from requirements.txt;

data – Runs python src/data/process_data.py; 

train – Runs python src/models/train.py; test – Runs pytest tests/; 

clean – Recursively removes every __pycache__ directory, removes .pytest_cache, and clears the contents of models/; 

all – Runs setup, data, train, and test in that order. All six target names must be declared as .PHONY so that Make never confuses them with files of the same name. 

After your changes, make all must complete without error. Makefile recipes must be indented with a real tab character, not spaces. Make rejects any recipe that is not tab-indented.



# Solution

## ✅ Corrected Makefile

```Makefile id="z8m3x6"
.PHONY: setup data train test clean all

setup:
	python3 -m venv mlops-venv
	mlops-venv/bin/pip install -r requirements.txt

data:
	python src/data/process_data.py

train:
	python src/models/train.py

test:
	pytest tests/

clean:
	find . -type d -name "__pycache__" -exec rm -rf {} +
	rm -rf .pytest_cache
	rm -rf models/*

all: setup data train test
```

<img width="961" height="627" alt="makefile" src="https://github.com/user-attachments/assets/17e47fec-bfbd-4875-b5d7-b6c0a7443a50" />


# Step-by-Step Execution

## 1. Navigate to the Project Directory

```bash id="r6v2k8"
cd /root/code/fraud-detection
```



## 2. Edit the Makefile

```bash id="y4p9m3"
vi Makefile
```

or

```bash id="t1x7q5"
nano Makefile
```



## 3. Save the Corrected Makefile

Ensure:

* Recipes use tabs
* `.PHONY` exists
* Cleanup works correctly



## 4. Run the Workflow

```bash id="u8m4r1"
make all
```

Expected execution flow:

```bash id="n5v2x9"
setup → data → train → test
```

<img width="656" height="545" alt="result" src="https://github.com/user-attachments/assets/1ce5ec8e-c71f-4c51-8b43-dd7fad3e6178" />


## 5. Optional Cleanup

```bash id="j7q3p8"
make clean
```



# Understanding the Concept 💡

## What is a Makefile?

A `Makefile` automates repetitive development tasks using simple commands.

Instead of manually running:

```bash id="h2x9m6"
python script.py
pytest
pip install
```

you can simply run:

```bash id="f4v7n2"
make all
```



## Why Use `.PHONY`?

`.PHONY` tells Make:

> “These are commands, not actual files.”

Without it, Make may skip execution if a file with the same name exists.



## Why Use Tabs?

Makefiles are extremely strict.

This works ✅:

```Makefile id="d8m1q4"
target:
	command
```

This fails ❌:

```Makefile id="w5x3r7"
target:
    command
```

because spaces are not tabs.



# Challenges Faced 🚧

* Space indentation causing Make errors
* Missing recursive cache cleanup
* Forgetting `.PHONY`
* Understanding Makefile execution order



# Key Learnings 📚

* Learned how Makefiles automate ML workflows
* Understood `.PHONY` targets
* Practiced proper Makefile syntax
* Learned recursive cleanup using `find`
* Explored workflow orchestration in MLOps projects



# Commands Used 🖥️

```bash id="a7m2v5"
cd /root/code/fraud-detection

vi Makefile

make all

make clean
```



# Fun Message 😄

*"Makefiles are like magic spells — one missing tab and suddenly nothing works 😄"*


