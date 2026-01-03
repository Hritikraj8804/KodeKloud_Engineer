# Instruction

As part of a data migration project, the team lead has tasked the team with migrating data from an existing S3 bucket to a new S3 bucket. The existing bucket contains a substantial amount of data that must be accurately transferred to the new bucket. The team is responsible for creating the new S3 bucket and ensuring that all data from the existing bucket is copied or synced to the new bucket completely and accurately. It is imperative to perform thorough verification steps to confirm that all data has been successfully transferred to the new bucket without any loss or corruption.

As a member of the Nautilus DevOps Team, your task is to perform the following:

Create a New Private S3 Bucket: Name the bucket `devops-sync-19208`.

Data Migration: Migrate the entire data from the existing `devops-s3-12582` bucket to the new `devops-sync-19208` bucket.

Ensure Data Consistency: Ensure that both buckets have the same data.


Use AWS CLI: Use the AWS CLI to perform the creation and data migration tasks.

# Solution

Migrating data between S3 buckets using the AWS CLI is an efficient way to ensure data consistency, especially when dealing with large datasets. The `sync` command is particularly powerful as it compares the source and destination to only copy new or modified files.

1. **Create the New S3 Bucket:**
Run the following command to create the private bucket in your default region:
```bash
aws s3 mb s3://devops-sync-19208

```


2. **Migrate and Sync Data:**
Use the `sync` command to copy all objects from the source to the destination. This command is preferred over `cp` for migrations because it verifies the data:
```bash
aws s3 sync s3://devops-s3-12582 s3://devops-sync-19208

```


*Note: This command performs a direct bucket-to-bucket copy within the AWS network, which is much faster than downloading and re-uploading.*
3. **Ensure Data Consistency:**
To verify that both buckets are identical, run the `ls` command with the `--recursive` and `--summarize` flags for both buckets and compare the object count and total size:
```bash
# Check source
aws s3 ls s3://devops-s3-12582 --recursive --human-readable --summarize

# Check destination
aws s3 ls s3://devops-sync-19208 --recursive --human-readable --summarize

```



### ✅ Verification Checklist

* [ ] Bucket `devops-sync-19208` is created and private.
* [ ] All objects from `devops-s3-12582` appear in the new bucket.
* [ ] The total number of files and byte count match between both buckets.

---

### Key Technical Details

| Feature | Command/Detail |
| --- | --- |
| **Sync vs Copy** | `sync` only transfers missing or modified files, making it faster for large migrations. |
| **Private Access** | The `s3api` creates buckets with "Block All Public Access" settings by default in modern AWS accounts. |
| **Integrity** | S3 uses MD5 checksums internally to ensure data is not corrupted during the transfer. |

---

### Critical Considerations

* **Permissions:** Your AWS CLI user must have `s3:ListBucket` and `s3:GetObject` on the source bucket, and `s3:PutObject` on the target bucket.
* **Versioning:** If the source bucket has Versioning enabled, the `sync` command will only copy the **latest** version of each object. To migrate all versions, specialized tools like S3 Replication or AWS DataSync are required.
* **Encryption:** If the source objects are encrypted with a KMS key, ensure the destination bucket has access to use that same key or re-encrypt during the sync.

---

### Performance Tip

> If you are migrating millions of small files, the sync command can be slow. You can increase the concurrency by running `aws configure set default.s3.max_concurrent_requests 20` to speed up the transfer!

[]()
