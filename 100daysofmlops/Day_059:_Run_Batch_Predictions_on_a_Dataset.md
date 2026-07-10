# Instruction

The xFusionCorp Industries ML Platform team processes overnight transaction batches using a pre-trained **RandomForest** fraud-detection model.

The provided `batch_predict.py` scaffold already defined the paths for the model, input data, and output file, but the entire batch scoring workflow was left as a **TODO**.

The objective was to:

* Load the trained model
* Read the batch input CSV
* Generate integer class predictions using `model.predict()`
* Append the predictions to the dataset
* Save the results as `predictions.csv`

# Solution

## Final `batch_predict.py`

```python
import joblib
import pandas as pd

MODEL_PATH = "/root/code/serving/model.pkl"
INPUT_CSV = "/root/code/serving/input.csv"
OUTPUT_CSV = "/root/code/serving/predictions.csv"

# Load the trained model
model = joblib.load(MODEL_PATH)

# Read the batch input
df = pd.read_csv(INPUT_CSV)

# Select feature columns
X = df[["amount", "hour", "num_tx_past_day"]]

# Generate integer class predictions
df["prediction"] = model.predict(X).astype(int)

# Save predictions
df.to_csv(OUTPUT_CSV, index=False)

print(f"Wrote {len(df)} rows to {OUTPUT_CSV}")
```

<img width="717" height="551" alt="todos" src="https://github.com/user-attachments/assets/e4567148-802b-40bb-8a26-267c7de2181f" />

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/serving
```

## Step 2 - Edit Script

```bash
vi batch_predict.py
```

Implement the batch prediction workflow.

## Step 3 - Run the Script

```bash
python3 batch_predict.py
```

Expected output:

```text
Wrote 10 rows to /root/code/serving/predictions.csv
```

<img width="892" height="82" alt="run" src="https://github.com/user-attachments/assets/262224c5-66f4-4209-88d4-13ffb4b98506" />

# Verification

## Verify Output File

```bash
ls -l predictions.csv
```

Expected:

```text
predictions.csv
```

## View Predictions

```bash
cat predictions.csv
```

Example:

```csv
amount,hour,num_tx_past_day,prediction
25.5,10,1,0
3200,23,5,1
890,2,3,0
...
```

<img width="967" height="441" alt="verify" src="https://github.com/user-attachments/assets/5cffd5d3-e6fe-4d5d-863e-4ba3b0941a6f" />

## Verify Row Count

```bash
wc -l input.csv predictions.csv
```

Both files should have the same number of data rows (predictions.csv has one extra column but the same number of records).

# Fun Message 😄

*"Real-world ML isn't just about predicting one record at a time—batch scoring lets models process thousands of transactions overnight with a single script! 🌙📊"*
