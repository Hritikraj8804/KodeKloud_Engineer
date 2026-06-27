# Instruction

The xFusionCorp Industries ML Platform team wanted to enforce **data quality checks** before training the fraud-detection model.

A **Great Expectations** project was already initialized with:

* A pandas datasource
* An empty expectation suite named `fraud_schema`
* A default checkpoint
* Data Docs configured to refresh after every validation

The task was to author four expectations, save the suite, execute the checkpoint, and verify the validation report in Data Docs.

# Why Great Expectations?

Instead of discovering bad data during model training:

```text id="eq5vho"
CSV
 │
 ▼
Training
 │
 ▼
Failure ❌
```

Great Expectations validates the dataset first:

```text id="tvz6cn"
CSV
 │
 ▼
Expectation Suite
 │
 ▼
Validation
 │
 ▼
Training ✅
```

This helps catch schema and data quality issues early in the pipeline.

# Solution

Inside the TODO section of `author_expectations.py`, add the following expectations.

## TODO 1 - Validate Required Columns

```python id="89jmv4"
suite.add_expectation(
    ge.ExpectTableColumnsToMatchSet(
        column_set=[
            "amount",
            "hour",
            "num_tx_past_day",
            "is_fraud",
        ],
    )
)
```

This ensures every batch contains exactly the required columns.

## TODO 2 - Validate Transaction Amount

```python id="oq5dgw"
suite.add_expectation(
    ge.ExpectColumnValuesToBeBetween(
        column="amount",
        min_value=0,
    )
)
```

Negative transaction amounts are not allowed.

## TODO 3 - Validate Hour

```python id="d64o8n"
suite.add_expectation(
    ge.ExpectColumnValuesToBeBetween(
        column="hour",
        min_value=0,
        max_value=23,
    )
)
```

Only valid 24-hour values are accepted.

## TODO 4 - Validate Fraud Label

```python id="7x2d2d"
suite.add_expectation(
    ge.ExpectColumnValuesToBeInSet(
        column="is_fraud",
        value_set=[0, 1],
    )
)
```

The target label must remain binary.

# Final TODO Block

```python id="e3d0oz"
suite.add_expectation(
    ge.ExpectTableColumnsToMatchSet(
        column_set=[
            "amount",
            "hour",
            "num_tx_past_day",
            "is_fraud",
        ],
    )
)

suite.add_expectation(
    ge.ExpectColumnValuesToBeBetween(
        column="amount",
        min_value=0,
    )
)

suite.add_expectation(
    ge.ExpectColumnValuesToBeBetween(
        column="hour",
        min_value=0,
        max_value=23,
    )
)

suite.add_expectation(
    ge.ExpectColumnValuesToBeInSet(
        column="is_fraud",
        value_set=[0, 1],
    )
)
```

# Solution Steps

## Step 1 - Edit the File

```bash id="26ymx6"
vi /root/code/dataquality/author_expectations.py
```

Insert the four expectations into the TODO section.

## Step 2 - Run the Script

```bash id="6yic5m"
python3 /root/code/dataquality/author_expectations.py
```

Expected output:

```text id="cdqqjh"
Persisted 4 expectations to fraud_schema

Checkpoint default result:
success=True
```

<img width="796" height="151" alt="run" src="https://github.com/user-attachments/assets/6ac7744b-d900-441d-80a6-beafcae13527" />

# Verification

## Verify Expectation Suite

```bash id="r35p6g"
cat /root/code/dataquality/gx/expectations/fraud_schema.json
```

The suite should contain:

```text id="l7g1d5"
expect_table_columns_to_match_set

expect_column_values_to_be_between
(amount)

expect_column_values_to_be_between
(hour)

expect_column_values_to_be_in_set
(is_fraud)
```

## Verify Validation Result

Check:

```bash id="3lmt11"
find /root/code/dataquality/gx/uncommitted/validations
```

Open the latest JSON.

Expected:

```json id="lmcd0b"
"success": true
```

## Verify Data Docs

Open:

```text id="5gblcq"
Data Docs
```

Expected:

Landing page shows:

```text id="b2j27l"
fraud_schema
```

Click the validation run.

Each expectation should display:

```text id="s8cpmp"
Success
```

with a green status.

<img width="1717" height="876" alt="ui" src="https://github.com/user-attachments/assets/c7765052-dee4-42c3-bb1f-342c3fcd1430" />

# Fun Message 😄

*"Great models start with great data. Great Expectations makes sure your dataset passes the interview before your model ever sees it! 📊✨"*
