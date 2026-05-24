## Instruction

The xFusionCorp Industries ML team uses **SeaweedFS** as a shared S3-compatible object store for DVC-tracked datasets.

A DVC repository already existed at:

```bash id="m7x3p1"
/root/code/fraud-detection/
```

with:

* DVC initialized ✅
* Dataset already tracked with DVC ✅

However:

```bash id="q4v8n1"
dvc push
```

was failing because the remote configuration inside:

```bash id="v2m8x5"
.dvc/config
```

was incorrect.

The task was to:

* Fix the DVC remote configuration
* Configure SeaweedFS as the default DVC remote
* Push tracked data into the shared object storage bucket



# Environment Details

| Setting     | Value                   |
| ----------- | ----------------------- |
| S3 Endpoint | `http://localhost:8333` |
| Bucket Name | `dvc-storage`           |
| Access Key  | `weedadmin`             |
| Secret Key  | `weedadmin123`          |

The bucket already existed under:

```bash id="k3v9n6"
/buckets/dvc-storage
```

inside the SeaweedFS Filer UI.



# Requirements

The DVC remote named:

```bash id="p7x4m1"
s3
```

needed to:

* Use `s3://dvc-storage`
* Use the correct endpoint URL
* Be configured as the default remote

After a successful push:

* The bucket must contain objects under:

```bash id="r5m2x8"
files/md5/
```



# Solution

## ✅ Step 1 - Navigate to the Repository

```bash id="u2m8x5"
cd /root/code/fraud-detection
```



# Step 2 - Inspect Existing DVC Configuration

View the current configuration:

```bash id="n4x7p2"
cat .dvc/config
```

The configuration likely had:

* Wrong bucket path
* Missing endpoint URL
* Incorrect remote settings
* No default remote



# Step 3 - Configure the Correct DVC Remote

Set the remote URL:

```bash id="c8m3x1"
dvc remote add -d s3 s3://dvc-storage
```

Explanation:

| Option           | Purpose                 |
| ---------------- | ----------------------- |
| -d               | Marks as default remote |
| s3               | Remote name             |
| s3://dvc-storage | Bucket path             |



# Step 4 - Configure SeaweedFS Endpoint

Set the custom S3 endpoint:

```bash id="j7x2m5"
dvc remote modify s3 endpointurl http://localhost:8333
```



# Step 5 - Configure Credentials

Set access key:

```bash id="t4v8n1"
dvc remote modify s3 access_key_id weedadmin
```

Set secret key:

```bash id="w9m3x6"
dvc remote modify s3 secret_access_key weedadmin123
```



# Step 6 - Verify the Final Configuration

Check:

```bash id="r5n1x8"
cat .dvc/config
```

Expected configuration:

```ini id="u8m4v2"
['remote "s3"']
    url = s3://dvc-storage
    endpointurl = http://localhost:8333
    access_key_id = weedadmin
    secret_access_key = weedadmin123
[core]
    remote = s3
```

<img width="901" height="360" alt="config" src="https://github.com/user-attachments/assets/39c584db-5cdf-4a91-9f2e-84ea882cae2f" />


# Step 7 - Push the Tracked Data

Run:

```bash id="a6x3m8"
dvc push
```

Expected output:

```bash id="z4x8m1"
1 file pushed
```



# Step 8 - Verify Objects in SeaweedFS

Open the SeaweedFS Filer UI using the lab button.

Navigate to:

```bash id="n5m2x7"
/buckets/dvc-storage/files/md5/
```

You should now see DVC-managed objects stored inside the bucket.


<img width="1568" height="322" alt="seawwd" src="https://github.com/user-attachments/assets/6197eb81-89c3-4778-9edf-b5df2556b074" />


# Understanding the Concept 💡

## Why Use Remote Storage with DVC?

DVC separates:

| Component  | Storage               |
| ---------- | --------------------- |
| Code       | Git                   |
| Large Data | Remote Object Storage |

This keeps repositories lightweight while allowing datasets and models to be versioned.



## Why SeaweedFS?

SeaweedFS provides:

* S3-compatible object storage
* Lightweight distributed storage
* Fast uploads/downloads
* Local cloud-like infrastructure

Perfect for MLOps labs and internal platforms.



## What Does `dvc push` Do?

It uploads tracked data into the configured remote storage backend.

Git tracks only metadata (`.dvc` files).



## Why Use `endpointurl`?

SeaweedFS is not AWS S3.

DVC needs a custom endpoint to know where the S3-compatible API lives.



# Challenges Faced 🚧

* Incorrect remote configuration
* Missing default remote
* Custom S3 endpoint confusion
* Understanding DVC remote storage concepts



# Key Learnings 📚

* Learned how DVC remote storage works
* Practiced configuring S3-compatible backends
* Explored SeaweedFS integration
* Understood object storage workflows in MLOps
* Learned how datasets move from local → remote storage



# Commands Used 🖥️

```bash id="b6x1m9"
cd /root/code/fraud-detection

cat .dvc/config

dvc remote add -d s3 s3://dvc-storage

dvc remote modify s3 endpointurl http://localhost:8333

dvc remote modify s3 access_key_id weedadmin

dvc remote modify s3 secret_access_key weedadmin123

cat .dvc/config

dvc push
```


<img width="776" height="317" alt="cmd" src="https://github.com/user-attachments/assets/6ac0e2ad-b185-4ded-8b79-41ea11e01d9d" />


# Fun Message 😄

*"Git stores your code, DVC stores your data, and SeaweedFS stores your storage bill 😄🌊"*

