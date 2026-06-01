## Instruction

The xFusionCorp Industries ML team is adopting **MLflow** for experiment tracking and model lifecycle management.

The task was to launch a local MLflow Tracking Server that could be used by the team's training pipelines to log:

* Parameters
* Metrics
* Artifacts
* Models

The server needed to remain running in the background and be accessible through the lab's MLflow UI button.



# Requirements

The MLflow server had to satisfy all of the following:

| Requirement        | Value                                 |
| ------------------ | ------------------------------------- |
| Port               | `5000`                                |
| Host Binding       | `0.0.0.0`                             |
| Backend Store      | `/root/code/mlflow-backend/mlflow.db` |
| Artifact Store     | `/root/code/mlflow-artifacts/`        |
| CORS Origins       | `*`                                   |
| Allowed Hosts      | `*`                                   |
| Background Process | Yes                                   |

Additionally:

* Parent directories must exist before startup
* The SQLite database file must be created automatically
* The MLflow UI button should open successfully

---

# Solution

##  Step 1 - Create Required Directories

MLflow will fail if the backend directory does not exist.

Create the directories:

```bash id="d1"
mkdir -p /root/code/mlflow-backend

mkdir -p /root/code/mlflow-artifacts
```

Verify:

```bash id="d2"
ls -ld /root/code/mlflow-backend

ls -ld /root/code/mlflow-artifacts
```

<img width="580" height="58" alt="files" src="https://github.com/user-attachments/assets/6bbd6d05-5123-4c1a-a1c7-d27c62af184b" />


# Step 2 - Start the MLflow Tracking Server

Launch MLflow in the background:

```bash id="d3"
nohup mlflow server \
  --host 0.0.0.0 \
  --port 5000 \
  --backend-store-uri sqlite:////root/code/mlflow-backend/mlflow.db \
  --default-artifact-root /root/code/mlflow-artifacts \
  --cors-allowed-origins '*' \
  --allowed-hosts '*' \
  > /tmp/mlflow.log 2>&1 &
```

### Explanation

| Option                       | Purpose                            |
| ---------------------------- | ---------------------------------- |
| `--host 0.0.0.0`             | Listen on all interfaces           |
| `--port 5000`                | Required port                      |
| `--backend-store-uri`        | SQLite metadata database           |
| `--default-artifact-root`    | Artifact storage location          |
| `--cors-allowed-origins '*'` | Allow proxy/browser requests       |
| `--allowed-hosts '*'`        | Accept all host headers            |
| `nohup ... &`                | Keep running after terminal closes |

<img width="665" height="176" alt="server" src="https://github.com/user-attachments/assets/edd7c57f-ed48-41bb-a778-d2d05fa3fdc8" />


# Step 3 - Verify the Process

Check that MLflow is running:

```bash id="d4"
ps -ef | grep mlflow
```

Expected output:

```text id="d5"
mlflow server --host 0.0.0.0 --port 5000 ...
```

<img width="792" height="418" alt="grep" src="https://github.com/user-attachments/assets/5656b64a-c53c-4679-9c09-be67ecf46a52" />


# Step 4 - Verify Port 5000

Check:

```bash id="d6"
ss -tulnp | grep 5000
```

Expected:

```text id="d7"
LISTEN 0 128 0.0.0.0:5000
```

<img width="802" height="115" alt="port" src="https://github.com/user-attachments/assets/830a8aa9-01e8-4c63-87bd-532439e6e14b" />


# Step 5 - Verify Database Creation

Check:

```bash id="d8"
ls -l /root/code/mlflow-backend/
```

Expected:

```text id="d9"
mlflow.db
```

The SQLite backend database is automatically created on first startup.

<img width="692" height="55" alt="file v" src="https://github.com/user-attachments/assets/16ac32f0-ad39-4e26-ac2a-7e03f6bdee8c" />


# Step 6 - Verify the UI

Open the **MLflow UI** button at the top of the lab.

Expected behavior:

* Dashboard loads successfully
* Default experiment is visible
* No CORS or host validation errors appear

At this point the experiment list will be empty because no runs have been logged yet.

<img width="1177" height="491" alt="mlflow" src="https://github.com/user-attachments/assets/1ccc93aa-fdfe-4f5e-9b5e-7c85e7fb3024" />

# Fun Message 😄

*"Git tracks code, DVC tracks data, and MLflow tracks all the experiments you forgot the details of 😄📊"*

