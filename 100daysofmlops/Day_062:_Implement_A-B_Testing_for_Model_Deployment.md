# Instruction

The xFusionCorp Industries ML Platform team introduced **A/B testing** for their fraud-detection service.

Two versions of the model are available:

* **MODEL_V1** → Stable production model (10-tree RandomForest)
* **MODEL_V2** → Candidate model (50-tree RandomForest)

The objective was to complete the routing logic inside `ab_server.py` so that:

* Approximately **80%** of incoming requests are served by **MODEL_V1**
* Approximately **20%** are served by **MODEL_V2**
* Every prediction response includes:

  * `is_fraud`
  * `model_version`

This allows downstream monitoring systems to compare model performance before fully promoting the new model.

# Solution

## Final `predict()` Function

```python
@app.route("/predict", methods=["POST"])
def predict():
    payload = request.get_json() or {}
    features = np.array([[
        float(payload.get("amount", 0.0)),
        int(payload.get("hour", 0)),
        int(payload.get("num_tx_past_day", 0)),
    ]])

    # Route approximately 80% of traffic to v1
    if random.random() < 0.8:
        model = MODEL_V1
        version = "v1"
    else:
        model = MODEL_V2
        version = "v2"

    prediction = int(model.predict(features)[0])

    return jsonify({
        "is_fraud": prediction,
        "model_version": version
    }), 200
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/serving
```

## Step 2 - Edit the Server

```bash
vi ab_server.py
```

Implement the A/B routing logic inside the `predict()` endpoint.

## Step 3 - Start the Server

```bash
python3 ab_server.py &
```

Expected output:

```text
* Running on http://0.0.0.0:8085
```

<img width="757" height="232" alt="ab_server" src="https://github.com/user-attachments/assets/48e702e3-8c29-474c-af69-ff75fadc7b0f" />

## Step 4 - Test the API

```bash
curl -X POST http://localhost:8085/predict \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
```

Example output:

```json
{
  "is_fraud": 1,
  "model_version": "v1"
}
```

or

```json
{
  "is_fraud": 1,
  "model_version": "v2"
}
```

# Verification

## Verify Health Endpoint

```bash
curl http://localhost:8085/health
```

Expected:

```json
{"status":"ok"}
```

## Verify Prediction

```bash
curl -X POST http://localhost:8085/predict \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
```

Example:

```json
{
  "is_fraud": 1,
  "model_version": "v1"
}
```

<img width="587" height="270" alt="test" src="https://github.com/user-attachments/assets/2869566e-7e7b-4305-b3eb-52d96efa207c" />

## Verify Traffic Split

Run approximately **200 requests**.

Example:

```bash
for i in {1..200}
do
curl -s -X POST http://localhost:8085/predict \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
echo
done
```

Expected distribution:

```text
v1 ≈ 160 requests (±20)

v2 ≈ 40 requests (±20)
```

<img width="522" height="187" alt="verify" src="https://github.com/user-attachments/assets/e09e400d-8926-4803-a4dc-996e51903489" />

# Fun Message 😄

*"Great ML deployments don't replace models overnight—they let the new model earn users' trust one prediction at a time through A/B testing! ⚖️🤖"*
