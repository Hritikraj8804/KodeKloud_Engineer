## Instruction

A new xFusionCorp Industries team member cloned the `fraud-detection` repository onto a fresh machine.

The repository already contained:

* DVC initialized ✅
* `.dvc` pointer files ✅
* Remote configuration ✅

However:

```bash id="m7x3p1"
dvc pull
```

was failing because authentication against the SeaweedFS remote storage was incomplete.

The dataset file:

```bash id="q4v8n1"
data/raw/transactions.csv
```

was missing locally and needed to be restored from the DVC remote.



# Environment Details

| Setting     | Value                   |
| ----------- | ----------------------- |
| S3 Endpoint | `http://localhost:8333` |
| Bucket      | `dvc-storage`           |
| Access Key  | `weedadmin`             |
| Secret Key  | `weedadmin123`          |



# Existing .dvc/config

```ini id="v2m8x5"
[core]
    remote = s3

['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
```


# Solution

## ✅ Step 1 - Navigate to the Repository

```bash id="k3v9n6"
cd /root/code/fraud-detection
```



# Step 2 - Add the Access Key

Run:

```bash id="p7x4m1"
dvc remote modify s3 access_key_id weedadmin
```



# Step 3 - Add the Secret Key

Run:

```bash id="r5m2x8"
dvc remote modify s3 secret_access_key weedadmin123
```



# Step 4 - Verify the Updated Configuration

Check:

```bash id="u2m8x5"
cat .dvc/config
```

Expected output:

```ini id="n4x7p2"
[core]
    remote = s3

['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
    access_key_id = weedadmin
    secret_access_key = weedadmin123
```

<img width="726" height="265" alt="config" src="https://github.com/user-attachments/assets/4d14b6f9-c591-4b22-9e04-b4d531e44862" />


# Step 5 - Pull the Dataset

Run:

```bash id="c8m3x1"
dvc pull
```

Expected output:

```bash id="j7x2m5"
1 file fetched and restored
```

This restores:

```bash id="t4v8n1"
data/raw/transactions.csv
```

from the remote object store.

<img width="1036" height="232" alt="dvcpull" src="https://github.com/user-attachments/assets/6f7f3ed7-0bfb-41bd-8d56-6e086aa9c1da" />


# Step 6 - Verify the Dataset Exists

Check:

```bash id="w9m3x6"
ls data/raw/
```

Expected output:

```bash id="r5n1x8"
transactions.csv
transactions.csv.dvc
```



# Step 7 - Validate the File Integrity

DVC automatically verifies the file hash against the checksum stored in:

```bash id="u8m4v2"
transactions.csv.dvc
```

This ensures the downloaded dataset matches the tracked version exactly.

<img width="852" height="252" alt="verfy" src="https://github.com/user-attachments/assets/26e52afd-ec8e-4863-9697-5c402537bc7f" />


# Understanding the Concept 💡

## What Does `dvc pull` Do?

`dvc pull`:

1. Reads `.dvc` pointer files
2. Connects to remote storage
3. Downloads missing datasets
4. Restores them into the workspace



## Why Are Credentials Required?

SeaweedFS provides S3-compatible storage.

Like AWS S3, authentication requires:

| Credential        | Purpose  |
| ----------------- | -------- |
| access_key_id     | Username |
| secret_access_key | Password |

Without them, DVC cannot access remote objects.



## Why Use Pointer Files?

Git tracks lightweight metadata:

```bash id="a6x3m8"
transactions.csv.dvc
```

while DVC handles the actual dataset storage separately.

This keeps repositories:

* Fast
* Lightweight
* Scalable



# Challenges Faced 🚧

* Missing authentication credentials
* Understanding DVC remote access
* Diagnosing object storage connection failures
* Learning DVC cache restoration workflows



# Key Learnings 📚

* Learned how DVC restores datasets from remote storage
* Practiced configuring S3-compatible authentication
* Understood DVC integrity verification
* Explored reproducible ML dataset workflows
* Learned the relationship between `.dvc` files and remote objects


# Fun Message 😄

*"DVC without credentials is basically showing up to cloud storage and saying: ‘Trust me bro.’ 😄"*
