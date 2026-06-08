## Instruction

The xFusionCorp Industries deployment team needed a batch prediction workflow built around the registered **champion** version of the `fraud-detector` model.

A custom preprocessing wrapper called:

```python
ScaledPredictor
```

was already implemented.

The task was to complete the MLflow integration so the script could:

* Load the champion model from the MLflow Model Registry
* Apply preprocessing
* Run batch predictions
* Generate a prediction CSV for downstream systems



# Solution

## ✅ TODO 1 - Load Champion Model

Replace:

```python
# TODO 1
```

with:

```python
inner_model = mlflow.pyfunc.load_model(MODEL_URI)
```



## ✅ TODO 2 - Run Batch Prediction

Replace:

```python
# TODO 2
```

with:

```python
predictions = predictor.predict(None, inputs.values)

inputs["prediction"] = predictions

inputs.to_csv(OUTPUT_CSV, index=False)
```



# Final Code Changes

## Model Loading

```python
inner_model = mlflow.pyfunc.load_model(MODEL_URI)
```



## Prediction Generation

```python
predictions = predictor.predict(None, inputs.values)

inputs["prediction"] = predictions

inputs.to_csv(OUTPUT_CSV, index=False)
```



# Step-by-Step Execution

## 1. Edit the Script

```bash
vi /root/code/predict_with_preprocessing.py
```

Add the two missing sections.

<img width="956" height="472" alt="todo" src="https://github.com/user-attachments/assets/8a316783-1454-4eba-8e08-165b080811de" />

## 2. Execute the Script

```bash
python3 /root/code/predict_with_preprocessing.py
```

Expected execution:

```text
(no errors)
```

<img width="762" height="127" alt="run" src="https://github.com/user-attachments/assets/09ed8dd4-519a-4869-845f-5500ee9cd680" />

## 3. Verify Output File

Check:

```bash
ls -l /root/code/predictions.csv
```

Expected:

```text
/root/code/predictions.csv
```

exists.

<img width="827" height="345" alt="csv" src="https://github.com/user-attachments/assets/273668ac-e227-45e6-92d0-0a5037e93f1d" />

## 4. Verify CSV Contents

View:

```bash
head /root/code/predictions.csv
```

Example:

```csv
feature1,feature2,feature3,prediction
1.2,0.5,3.4,0
2.1,1.7,4.5,1
...
```

<img width="547" height="197" alt="verify" src="https://github.com/user-attachments/assets/893b7ee6-7d88-4e79-b673-4559c325ba6e" />

# Validation Checks ✅

## Header Exists

```csv
prediction
```

column present.



## Row Count

Input file:

```bash
wc -l /root/code/data/inputs.csv
```

Expected:

```text
11
```

(1 header + 10 rows)

Output:

```bash
wc -l /root/code/predictions.csv
```

Expected:

```text
11
```

Same number of prediction rows.



## Prediction Column Exists

Verify:

```bash
csvcut -n /root/code/predictions.csv
```

or

```bash
head -1 /root/code/predictions.csv
```

Expected:

```text
prediction
```

among the columns.


# Fun Message 😄

*"Training a model is only half the journey. Making predictions with it is where the business finally gets its answers 😄📊"*
