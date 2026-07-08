# Instruction

The xFusionCorp Industries ML Platform team serves the fraud-detection model through a Flask REST API so downstream services can score transactions over HTTP.

The application exposes two endpoints:

* **GET `/health`** — Health check
* **POST `/predict`** — Predict whether a transaction is fraudulent

The provided `app.py` had two issues:

* The server listened on the wrong port.
* `/predict` ignored the JSON request body and always used default values.

# Solution

Update the API to read JSON from the request body and run on port **8085**.

## Final `app.py`

```python
import joblib
import numpy as np
from flask import Flask, jsonify, request

MODEL_PATH = "/root/code/serving/model.pkl"
MODEL = joblib.load(MODEL_PATH)

app = Flask(__name__)


@app.route("/health")
def health():
    return jsonify({"status": "ok"}), 200


@app.route("/predict", methods=["POST"])
def predict():
    data = request.get_json() or {}

    amount = float(data.get("amount", 0.0))
    hour = int(data.get("hour", 0))
    num_tx_past_day = int(data.get("num_tx_past_day", 0))

    features = np.array([[amount, hour, num_tx_past_day]])

    is_fraud = int(MODEL.predict(features)[0])

    return jsonify({"is_fraud": is_fraud}), 200


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8085)
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/serving
```

## Step 2 - Edit Application

```bash
vi app.py
```

Make two changes:

* Replace `request.args` with `request.get_json()`
* Change port `5000` → `8085`

## Step 3 - Start Flask Server

```bash
python3 app.py &
```

<img width="677" height="65" alt="run" src="https://github.com/user-attachments/assets/39098d12-55e4-474b-b2b5-7892d197fe80" />

Expected:

```text
* Running on http://0.0.0.0:8085
```

# Verification

## Verify Health Endpoint

```bash
curl http://localhost:8085/health
```

Expected:

```json
{
  "status": "ok"
}
```

<img width="732" height="102" alt="curl" src="https://github.com/user-attachments/assets/233e06ac-3f0f-4541-828c-b1136c1a4599" />

## Verify High-Risk Prediction

```bash
curl -X POST http://localhost:8085/predict \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
```

Expected:

```json
{
  "is_fraud": 1
}
```

## Verify Low-Risk Prediction

```bash
curl -X POST http://localhost:8085/predict \
-H "Content-Type: application/json" \
-d '{"amount":25.5,"hour":10,"num_tx_past_day":1}'
```

Expected:

```json
{
  "is_fraud": 0
}
```

<img width="737" height="457" alt="verify" src="https://github.com/user-attachments/assets/cd19b2f4-b3c1-4672-96a3-ec2b68ca6a44" />

The important requirement is that the two payloads return **different predictions**, confirming the API reads the request body correctly.

# Fun Message 😄

*"A machine learning model is only useful when others can talk to it. Wrapping it in a Flask API turns a `.pkl` file into a service that any application can use! 🌐🤖"*
