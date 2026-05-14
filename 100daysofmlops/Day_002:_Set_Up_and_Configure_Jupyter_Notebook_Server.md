
## Instruction

A teammate has configured a JupyterLab server for the xFusionCorp Industries data science team, but the server is not behaving correctly.

The task was to inspect the configuration, diagnose the issues, correct the settings, and successfully start the JupyterLab server.

### Requirements

The running JupyterLab server must:

* Listen on port `8888`
* Bind on `0.0.0.0`
* Use `/root/notebooks/` as the notebook root directory
* Ensure `/root/notebooks/` exists on disk

The configuration file was located at:

```bash id="g5f6r1"
/root/code/jupyter_lab_config.py
```

JupyterLab also had to be started using:

```bash id="m2k7t9"
source /root/code/ml-env/bin/activate

jupyter lab --config=/root/code/jupyter_lab_config.py --allow-root --no-browser &
```



# Solution

## ✅ Steps to Complete the Task

### 1. Navigate to the Configuration File

```bash id="t7h2d4"
cd /root/code
```

Open the configuration file using any editor:

```bash id="p9x4n2"
vi jupyter_lab_config.py
```

or

```bash id="l3q8s5"
nano jupyter_lab_config.py
```



### 2. Inspect and Fix Incorrect Settings

The following settings needed correction:

| Requirement        | Correct Value      |
| ------------------ | ------------------ |
| Port               | `8888`             |
| IP Binding         | `0.0.0.0`          |
| Notebook Directory | `/root/notebooks/` |

Updated configuration:

```python id="n8v5b1"
c.ServerApp.port = 8888
c.ServerApp.ip = '0.0.0.0'
c.ServerApp.root_dir = '/root/notebooks/'
```

<img width="1047" height="440" alt="config" src="https://github.com/user-attachments/assets/af7a47c8-9a23-41c7-938f-1d8fd113286d" />


### 3. Create the Notebook Directory

The notebook root directory must exist before starting JupyterLab.

```bash id="y4m1z7"
mkdir -p /root/notebooks
```

Verify:

```bash id="u6r3w8"
ls -ld /root/notebooks
```



### 4. Activate the Python Virtual Environment

```bash id="f2k9p6"
source /root/code/ml-env/bin/activate
```

The prompt should now show:

```bash id="h8n4c2"
(ml-env) root@controlplane:~#
```



### 5. Start JupyterLab

Run JupyterLab using the corrected configuration:

```bash id="c5x7d9"
jupyter lab --config=/root/code/jupyter_lab_config.py --allow-root --no-browser &
```

<img width="750" height="510" alt="cmd" src="https://github.com/user-attachments/assets/f093187a-e9a8-4477-beed-294d6385f2a4" />

<img width="756" height="231" alt="cmd run" src="https://github.com/user-attachments/assets/ec2cd25c-382e-4a9c-b8c5-e31bc95216ad" />


### 6. Verify JupyterLab is Running

Check the running process:

```bash id="v3b8n1"
ps -ef | grep jupyter
```

You can also verify that port `8888` is listening:

```bash id="k7m2q4"
ss -tulnp | grep 8888
```

Expected output should show:

```bash id="j9r5p8"
0.0.0.0:8888
```
<img width="857" height="636" alt="jupyter" src="https://github.com/user-attachments/assets/a6f63a43-5592-4f3f-8c57-9f5e3a801696" />



# Understanding the Concept 💡

## Why `0.0.0.0`?

Binding to:

```bash id="z2x6v3"
127.0.0.1
```

only allows local access.

Using:

```bash id="b4p1r7"
0.0.0.0
```

allows external connections, which is required for lab proxies and browser access.



## Why Port 8888?

`8888` is the default port commonly used by Jupyter Notebook and JupyterLab services.



## What is `root_dir`?

The `root_dir` defines the default directory where notebooks are stored and accessed from the Jupyter interface.



# Challenges Faced 🚧

* Incorrect IP binding preventing browser access
* Wrong notebook directory path
* Missing `/root/notebooks` directory
* Forgetting to activate the virtual environment before running JupyterLab



# Key Learnings 📚

* Learned how JupyterLab configuration works
* Understood the importance of network binding (`0.0.0.0`)
* Practiced troubleshooting server configuration issues
* Learned how to run JupyterLab in the background
* Explored how notebook root directories are managed



# Commands Used 🖥️

```bash id="a6q9w3"
cd /root/code

vi jupyter_lab_config.py

mkdir -p /root/notebooks

source /root/code/ml-env/bin/activate

jupyter lab --config=/root/code/jupyter_lab_config.py --allow-root --no-browser &

ps -ef | grep jupyter

ss -tulnp | grep 8888
```



# Fun Message 😄

*"Nothing teaches debugging better than fixing someone else’s broken configuration at 2 AM 😄"*



