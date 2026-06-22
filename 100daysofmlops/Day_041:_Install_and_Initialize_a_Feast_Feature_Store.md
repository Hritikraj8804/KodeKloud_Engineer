# Instruction

The xFusionCorp Industries ML Platform team began adopting **Feast** as their centralized feature store for the fraud-detection platform.

The objective was to:

* Create a new Feast feature repository
* Generate the default Feast scaffold
* Apply the registry definitions
* Launch the Feast UI
* Verify the project appears in the dashboard

Feast was already installed on the system.

# What is Feast? 🤔

Feast is an open-source feature store that helps ML teams:

* Manage feature definitions
* Share features across models
* Maintain offline and online feature stores
* Ensure training and serving consistency

Architecture:

```text
Feature Definitions
        │
        ▼
   Feast Registry
        │
        ▼
 Offline Store
        │
        ▼
 Online Store
        │
        ▼
 Model Serving
```

# Solution Steps

## Step 1 - Verify Feast Installation

```bash
feast version
```

Expected:

```text
Feast Version: x.x.x
```

<img width="516" height="66" alt="fast ver" src="https://github.com/user-attachments/assets/821979dc-7bf3-4cc2-b7c8-063f01f3977e" />

The command should exit successfully.

## Step 2 - Initialize a New Feature Repository

Move into the working directory:

```bash
cd /root/code
```

Initialize the repository:

```bash
feast init feature_repo
```

Expected structure:

```text
/root/code/feature_repo/
└── feature_repo/
    ├── feature_store.yaml
    ├── feature_definitions.py
    ├── data/
    └── example_repo.py
```

<img width="712" height="262" alt="repo str" src="https://github.com/user-attachments/assets/cfca5f64-bac0-4d40-8f30-7abdb4cef4ac" />

## Step 3 - Enter the Feast Repository

```bash
cd /root/code/feature_repo/feature_repo
```

Verify files:

```bash
ls
```

Expected:

```text
feature_store.yaml
feature_definitions.py
data/
```

## Step 4 - Apply the Registry

Run:

```bash
feast apply
```

<img width="741" height="171" alt="fast apply" src="https://github.com/user-attachments/assets/aa657e12-8f70-4f20-8d5a-05f96497738f" />

Expected output:

```text
Created entity ...
Created feature view ...
Registry updated successfully
```

This command creates:

```text
data/registry.db
```

## Step 5 - Verify Registry

```bash
ls data/
```

Expected:

```text
registry.db
```

# Validation Checks

## Verify feature_store.yaml

```bash
cat feature_store.yaml
```

Expected keys:

```yaml
project:
provider:
registry:
```

Example:

```yaml
project: feature_repo
provider: local
registry: data/registry.db
```

## Verify Registry File

```bash
ls -l data/registry.db
```

Expected:

```text
registry.db
```

exists.

# Launch Feast UI

Since Feast UI is a long-running process:

Open a second terminal or run in background.

```bash
feast ui &
```

or

```bash
nohup feast ui > feast-ui.log 2>&1 &
```

Verify:

```bash
ps -ef | grep feast
```

Expected:

```text
feast ui
```

<img width="755" height="417" alt="feature ui" src="https://github.com/user-attachments/assets/23a8b890-4ecf-432d-9445-65ef03bb7450" />

running.

# Verify UI

Click:

```text
Feast UI
```

button in the lab.

Expected:

```text
Responsive Dashboard
```

showing:

```text
feature_repo
```

<img width="1852" height="597" alt="feast ui" src="https://github.com/user-attachments/assets/68a01b2d-dbd0-437d-8d5b-cb97e21a3361" />

# Final Directory Structure

```text
/root/code/feature_repo/
└── feature_repo/
    ├── feature_store.yaml
    ├── feature_definitions.py
    ├── data/
    │   └── registry.db
    └── ...
```

# Fun Message 😄

*"Models may come and go, but features live forever. That's why we built a feature store 😄🏪⚡"*
