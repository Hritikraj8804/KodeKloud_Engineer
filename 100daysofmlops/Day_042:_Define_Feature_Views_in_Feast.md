# Instruction

The xFusionCorp Industries ML Platform team stores fraud-detection features in a Feast repository.

A draft feature definition file existed at:

```bash
/root/code/fraud-detection/feature_repo/features.py
```

However, the registry schema no longer matched the actual source dataset:

```text
data/transactions.parquet
```

The task was to:

* Fix the Entity join key
* Correct the feature datatype
* Re-apply the Feast registry
* Verify the updated schema in the Feast UI

# Understanding the Dataset

The source parquet file contains:

| Column          | Type      |
| --------------- | --------- |
| customer_id     | Key       |
| amount          | Float32   |
| hour            | Int64     |
| num_tx_past_day | Int64     |
| is_fraud        | Int64     |
| event_timestamp | Timestamp |

# Solution Steps


# Final Corrected Section

```python
customer = Entity(
    name="customer",
    join_keys=["customer_id"],
    description="Customer identifier keyed by the transactions source.",
)

customer_transaction_features = FeatureView(
    name="customer_transaction_features",
    entities=[customer],
    ttl=timedelta(days=365),
    schema=[
        Field(name="amount", dtype=Float32),
        Field(name="hour", dtype=Int64),
        Field(name="num_tx_past_day", dtype=Int64),
    ],
    source=transactions_source,
    online=True,
)
```


## Step 1 - Edit features.py

```bash
cd /root/code/fraud-detection/feature_repo

vi features.py
```

Apply both fixes.

## Step 2 - Rebuild Feast Registry

Run:

```bash
feast apply
```

Expected output:

```text
Applying repository...
Created/Updated Entity customer
Created/Updated FeatureView customer_transaction_features
```

<img width="732" height="452" alt="run" src="https://github.com/user-attachments/assets/8fb6ebc0-4eb8-4b30-9622-968188a2524b" />

No errors should occur.

## Step 3 - Verify Registry

```bash
ls -l data/registry.db
```

Expected:

```text
registry.db
```

exists.

# Verification

## Verify Entity Join Key

Run:

```bash
feast entities list
```

Expected:

```text
customer
```

with:

```text
join_keys:
  - customer_id
```

<img width="1580" height="802" alt="entity ui" src="https://github.com/user-attachments/assets/3f6d7598-7a81-4772-89c6-ec51e73f9549" />

## Verify Feature View

Run:

```bash
feast feature-views list
```

Expected:

```text
customer_transaction_features
```

## Verify Schema

The feature view should contain:

```text
amount           Float32
hour             Int64
num_tx_past_day  Int64
```
<img width="1617" height="760" alt="customer_tran ui" src="https://github.com/user-attachments/assets/e7ce8eec-f83a-4e12-9e7a-9845058a4460" />

# Verify Feast UI

Open:

```text
Feast UI
```

Navigate to:

```text
Entities
```

Expected:

```text
customer
join_keys = ["customer_id"]
```

Navigate to:

```text
Feature Views
```

Expected:

```text
customer_transaction_features
```

Schema:

```text
amount           Float32
hour             Int64
num_tx_past_day  Int64
```

# Fun Message 😄

*"A feature store is only as smart as its join key. If Feast looks for `id` while your data speaks `customer_id`, they're having two completely different conversations 😄🏪"*
