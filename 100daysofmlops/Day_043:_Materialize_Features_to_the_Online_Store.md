# Instruction

The xFusionCorp Industries ML Platform team uses Feast to manage fraud-detection features.

The feature repository was already configured correctly:

* Registry applied successfully
* Feature View defined correctly
* SQLite online store configured
* Source data available in Parquet format

However, the materialization script:

```bash
/root/code/fraud-detection/feature_repo/materialize.sh
```

was not loading any rows into the online store.

The issue was that the materialization end date was set before any source events existed.

# Solution Steps

Replace:

```bash
END_DATE="1970-12-31T00:00:00"
```

with an ISO-8601 date after the latest source event.

Example:

```bash
END_DATE="2025-12-31T23:59:59"
```

## Step 1 - Edit Script

```bash
cd /root/code/fraud-detection/feature_repo

vi materialize.sh
```

Update:

```bash
END_DATE="2025-12-31T23:59:59"
```

Save.

## Step 2 - Make Sure Script is Executable

```bash
chmod +x materialize.sh
```

## Step 3 - Run Materialization

```bash
./materialize.sh
```

Expected output similar to:

```text
Materializing feature views...
customer_transaction_features
...
```

<img width="740" height="172" alt="run" src="https://github.com/user-attachments/assets/0a26261e-59a5-4037-b6dc-c3c370204a9d" />

No errors should occur.

# Verification

## Verify Online Store Exists

```bash
ls -lh data/online_store.db
```

Expected:

```text
online_store.db
```

exists.

Size should be larger than:

```text
4 KB
```

indicating actual feature data has been written.

Example:

```text
12K
16K
20K
```

or larger.

<img width="737" height="82" alt="verify cli" src="https://github.com/user-attachments/assets/764e6209-9662-4ac9-abfa-7814097d048c" />

## Verify Features from Feast

Launch Python:

```bash
python3
```

Run:

```python
from feast import FeatureStore

store = FeatureStore(repo_path=".")

features = store.get_online_features(
    features=[
        "customer_transaction_features:amount",
        "customer_transaction_features:hour",
        "customer_transaction_features:num_tx_past_day",
    ],
    entity_rows=[
        {"customer_id": 1},
        {"customer_id": 2},
        {"customer_id": 3},
    ],
).to_dict()

print(features)
```

Expected:

```python
{
    "customer_id": [1, 2, 3],
    "amount": [125.50, 340.75, 89.10],
    "hour": [12, 8, 22],
    "num_tx_past_day": [3, 1, 5]
}
```

Values vary, but:

```text
amount != None
```

for at least one customer.

# Verify Materialization Range

Check script:

```bash
cat materialize.sh
```

Expected:

```bash
END_DATE="2025-12-31T23:59:59"
```

or any date:

```text
>= 2024-01-09
```


# Fun Message 😄

*"A feature store without materialization is like a warehouse full of products with no delivery trucks. The data exists—you just can't get it where it needs to go 😄🚚🏪⚡"*
