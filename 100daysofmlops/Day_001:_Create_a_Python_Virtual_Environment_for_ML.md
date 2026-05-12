# Instruction

The xFusionCorp Industries data science team needs a standardised Python environment for their new ML project. Set up a virtual environment with the required ML libraries on the controlplane host.


Create a Python virtual environment named ml-env under /root/code/ using python3 -m venv.

Activate the environment and install the following packages: numpy, pandas, scikit-learn, and matplotlib.

Generate a requirements.txt file using pip freeze and save it at /root/code/requirements.txt.

# Solution

## ✅ Steps to Complete the Task

### 1. Navigate to the Working Directory

```bash
cd /root/code
```



### 2. Create the Python Virtual Environment

```bash
python3 -m venv ml-env
```

<img width="572" height="102" alt="env" src="https://github.com/user-attachments/assets/2c35f411-069b-42fd-b9c1-75ab86d8c8c1" />

This creates an isolated Python environment named `ml-env`.



### 3. Activate the Virtual Environment

```bash
source ml-env/bin/activate
```

After activation, your terminal prompt will look something like this:

```bash
(ml-env) root@controlplane:~#
```



### 4. Install Required ML Libraries

```bash
pip install numpy pandas scikit-learn matplotlib
```

Installed libraries:

| Package      | Purpose                        |
| ------------ | ------------------------------ |
| numpy        | Numerical computations         |
| pandas       | Data analysis and manipulation |
| scikit-learn | Machine learning library       |
| matplotlib   | Data visualization             |

<img width="802" height="352" alt="lib install" src="https://github.com/user-attachments/assets/070dd172-1944-4cd0-bdd6-c3e392ec2f76" />



### 5. Generate requirements.txt

```bash
pip freeze > /root/code/requirements.txt
```

<img width="1021" height="720" alt="freeze" src="https://github.com/user-attachments/assets/e704411b-6276-483e-8d6d-761ccd64d87f" />

This stores all installed package versions in the `requirements.txt` file.



### 6. Verify the File

```bash
cat /root/code/requirements.txt
```

Example output:

```txt
matplotlib==3.x.x
numpy==2.x.x
pandas==2.x.x
scikit-learn==1.x.x
```



# Understanding the Concept 💡

A **Python virtual environment** helps isolate project dependencies so that different projects can use different package versions without conflicts.

Using `requirements.txt` makes projects:

* Reproducible
* Easy to share
* Easy to deploy

Any developer can recreate the same environment using:

```bash
pip install -r requirements.txt
```



# Challenges Faced 🚧

* Forgetting to activate the virtual environment before installing packages.
* Installing packages globally instead of inside the virtual environment.
* Missing package versions in the `requirements.txt` file.



# Key Learnings 📚

* Learned how to create isolated Python environments using `venv`.
* Understood how dependency management works in Python projects.
* Explored the importance of `requirements.txt` in ML workflows.
* Practiced setting up a clean ML development environment.



# Fun Message 😄

*"A virtual environment is basically giving your project its own room so dependencies stop fighting with each other!"*



# Commands Used 🖥️

```bash
cd /root/code

python3 -m venv ml-env

source ml-env/bin/activate

pip install numpy pandas scikit-learn matplotlib

pip freeze > /root/code/requirements.txt
```



