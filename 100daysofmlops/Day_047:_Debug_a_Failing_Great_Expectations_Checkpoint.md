# Instruction

The xFusionCorp Industries ML Platform team extended the existing **fraud_schema** expectation suite to validate a new production batch:

```text id="6ijtzj"
data/transactions_drifted.csv
```

The baseline dataset had always enforced:

```text id="d56b1z"
amount >= 0
```

However, the new production data introduced a legitimate business change:

* Negative transaction amounts now represent **customer refunds**.

As a result, the **drift_check** checkpoint failed.

The objective was to:

* Investigate the failure using **Data Docs**
* Identify the failing expectation
* Widen the validation rule (instead of removing it)
* Re-run the checkpoint
* Confirm the validation passed


# Solution

Replace:

```python id="20a9v0"
suite.add_expectation(
    ge.ExpectColumnValuesToBeBetween(
        column="amount",
        min_value=0
    )
)
```

with:

```python id="chlr5m"
suite.add_expectation(
    ge.ExpectColumnValuesToBeBetween(
        column="amount",
        min_value=-400
    )
)
```

This allows legitimate refunds while still preventing excessively negative values.

# Updated Expectation

```python id="u5tbdc"
suite.add_expectation(
    ge.ExpectColumnValuesToBeBetween(
        column="amount",
        min_value=-400
    )
)
```

# Solution Steps

## Step 1 - Inspect Data Docs

Open:

```text id="9j8owx"
Data Docs
```

Navigate to:

```text id="gxjlwm"
fraud_schema
    ↓
drift_check
```

The failed validation highlights:

* Column:

  * `amount`
* Failed expectation:

  * `ExpectColumnValuesToBeBetween`
* Observed minimum value:

  * around **-400**

## Step 2 - Edit the Script

```bash id="jsr6uq"
vi /root/code/dataquality/fix_drift.py
```

Update the lower bound:

```python id="ydj86s"
min_value=-400
```

## Step 3 - Run the Script

```bash id="5h3h0v"
python3 /root/code/dataquality/fix_drift.py
```

Expected output:

```text id="2pdttm"
Persisted 4 expectations to fraud_schema

Checkpoint drift_check result:
success=True
```

<img width="737" height="106" alt="run" src="https://github.com/user-attachments/assets/e66a5099-e7d3-4c80-a27d-279826d9b77d" />

# Verification

## Verify Expectation Suite

```bash id="r4gl7l"
cat /root/code/dataquality/gx/expectations/fraud_schema.json
```

The suite should still contain all four expectation types:

* `expect_table_columns_to_match_set`
* `expect_column_values_to_be_between` (amount)
* `expect_column_values_to_be_between` (hour)
* `expect_column_values_to_be_in_set`

Only the lower bound for `amount` changes.

<img width="740" height="210" alt="files verify" src="https://github.com/user-attachments/assets/e133da97-a1e6-4e2c-9548-a0e5dee8399e" />

## Verify Validation Result

Check the latest validation:

```bash id="vl3xiz"
find /root/code/dataquality/gx/uncommitted/validations
```

Open the newest JSON.

Expected:

```json id="rbgdrw"
"success": true
```

## Verify Data Docs

Refresh:

```text id="yjpnvw"
Data Docs
```

Open:

```text id="0sdh3y"
fraud_schema
```

Latest:

```text id="v2bxgk"
drift_check
```

should now be:

```text id="xgmyqm"
Success
```

<img width="1877" height="582" alt="ui rows" src="https://github.com/user-attachments/assets/357737a7-51b9-41fd-8de8-f008cc1ca30d" />

<img width="1866" height="847" alt="ui" src="https://github.com/user-attachments/assets/962e6680-6a18-4085-bc95-40f6df848a0b" />

# Fun Message 😄

*"Data drift isn't always a bug—sometimes it's your business evolving. The trick is teaching your validation rules the new reality without letting bad data sneak through! 📊🚀"*
