#Instruction

The xFusionCorp Industries ML platform team has deployed the fraud-detection model using BentoML. The model is registered in BentoML's local store and is served over HTTP with the command bentoml serve, which automatically generates a Swagger UI at the server's root. Within the scaffold located at /root/code/serving/service.py, the modern @bentoml.service class API is utilized. This script loads the pre-registered fraud_detector:latest model from the store and defines the APIs, although the POST /predict handler remains unimplemented. Your objective is to implement the /predict handler to score a transaction using the loaded model. Additionally, you need to start the server on port 3000 and verify that it returns predictions.


The fraud_detector model is registered in BentoML's local store. The BentoML server is NOT pre-started — the BentoML UI button opens the Swagger surface once the server is running on port 3000.

The project layout under /root/code/serving/:

service.py – BentoML service (@bentoml.service class FraudService). The model-store load (bentoml.models.BentoModel + bentoml.sklearn.load_model in __init__) and the last_predictions API are wired. The predict handler body is left as a TODO — it returns an error until authored. The handler takes the amount/hour/num_tx_past_day parameters and returns {"is_fraud": <int>}.
train.csv – The 10-row source used at startup to train and register the model with bentoml.sklearn.save_model("fraud_detector", model).
The end state must include:

bentoml models list lists fraud_detector in the store.
curl http://localhost:3000/ returns HTTP 200 – The Swagger UI is reachable once the server is running.
POST /predict with a valid payload returns {"is_fraud": 0} or {"is_fraud": 1}.
Two distinct payloads return different is_fraud values – The handler scores the posted features.
Suggested payloads: {"amount": 3200, "hour": 23, "num_tx_past_day": 5} (high-value, late-night—expected to flag fraud); {"amount": 25.5, "hour": 10, "num_tx_past_day": 1} (low-value, daytime).


For this lab, you only need to implement the `predict()` API, start the BentoML service, and verify it with two different requests.

## 1. Update `service.py`

Replace the `predict()` function with:

```python
@bentoml.api
def predict(
    self, amount: float, hour: int, num_tx_past_day: int
) -> Dict[str, Any]:

    features = np.array([[amount, hour, num_tx_past_day]])

    is_fraud = int(self.model.predict(features)[0])

    self._history.append({
        "amount": amount,
        "hour": hour,
        "num_tx_past_day": num_tx_past_day,
        "is_fraud": is_fraud,
    })

    return {"is_fraud": is_fraud}
```


## 2. Verify the model exists

```bash
bentoml models list
```

Expected output should include:

```
fraud_detector:latest
```

## 3. Start the BentoML server

From the project directory:

```bash
cd /root/code/serving
```

Run:

```bash
bentoml serve service:FraudService --port 3000 &
```

or

```bash
nohup bentoml serve service:FraudService --port 3000 >/tmp/bento.log 2>&1 &
```

<img width="737" height="127" alt="model list" src="https://github.com/user-attachments/assets/64315723-dc24-4992-be36-5d95090c2c48" />

## 4. Verify Swagger UI

```bash
curl -I http://localhost:3000/
```

Expected:

```
HTTP/1.1 200 OK
```

<img width="542" height="185" alt="curl" src="https://github.com/user-attachments/assets/f36bf5d8-08d9-4c3e-881b-b75291c1d8f1" />

You can also open the **BentoML UI** button in the lab.

## 5. Test Prediction (Fraud)

```bash
curl -X POST http://localhost:3000/predict \
-H "Content-Type: application/json" \
-d '{
      "amount":3200,
      "hour":23,
      "num_tx_past_day":5
    }'
```

Example output:

```json
{
  "is_fraud": 1
}
```

## 6. Test Prediction (Normal)

```bash
curl -X POST http://localhost:3000/predict \
-H "Content-Type: application/json" \
-d '{
      "amount":25.5,
      "hour":10,
      "num_tx_past_day":1
    }'
```

Example output:

```json
{
  "is_fraud": 0
}
```

The two payloads should produce different predictions.

## 7. Verify Prediction History

```bash
curl -X POST http://localhost:3000/last_predictions
```

Example:

```json
{
  "count": 2,
  "predictions": [
    {
      "amount": 3200,
      "hour": 23,
      "num_tx_past_day": 5,
      "is_fraud": 1
    },
    {
      "amount": 25.5,
      "hour": 10,
      "num_tx_past_day": 1,
      "is_fraud": 0
    }
  ]
}
```

<img width="602" height="342" alt="test" src="https://github.com/user-attachments/assets/fefe7aff-cd09-4b83-9471-952fc9c6d339" />

<img width="1082" height="795" alt="ui" src="https://github.com/user-attachments/assets/b3e37ced-7092-4160-91fb-9aa9bda6092c" />
