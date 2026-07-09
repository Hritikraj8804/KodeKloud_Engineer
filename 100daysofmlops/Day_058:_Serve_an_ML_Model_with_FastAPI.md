# Instruction

The xFusionCorp Industries ML Platform team exposes the fraud-detection model through a **FastAPI** service. Instead of using `curl`, engineers interact with the API using the automatically generated **Swagger UI** (`/docs`).

The application is already running and records every prediction in an in-memory audit log. The objective is to submit three different prediction requests through the Swagger UI and verify they were recorded successfully.

# Solution

Since the FastAPI application is already running, **no code changes are required**. The task is completed entirely through the Swagger UI.

# Solution Steps

## Step 1 - Open Swagger UI

Open the browser using the **FastAPI Swagger UI** button (or visit):

```text
http://localhost:8085/docs
```

You should see the automatically generated API documentation.

## Step 2 - Expand POST `/predict`

Locate:

```text
POST /predict
```

Click:

```text
Try it out
```

## Step 3 - Execute Prediction #1

Use the following JSON:

```json
{
  "amount": 3200,
  "hour": 23,
  "num_tx_past_day": 5
}
```

Click:

```text
Execute
```

<img width="1471" height="377" alt="1 1" src="https://github.com/user-attachments/assets/d23394ba-77e9-47b9-bf2b-8fe776e88022" />

Expected response:

```json
{
  "is_fraud": 1
}
```

<img width="1497" height="692" alt="1 2" src="https://github.com/user-attachments/assets/3898030c-42ed-4cc4-9f4c-c33393c8103b" />

<img width="1436" height="647" alt="1 3" src="https://github.com/user-attachments/assets/aeead5ee-c0be-45dc-8a0e-ab51cb77158e" />

## Step 4 - Execute Prediction #2

```json
{
  "amount": 25.5,
  "hour": 10,
  "num_tx_past_day": 1
}
```

Click **Execute**.

Expected response:

```json
{
  "is_fraud": 0
}
```

## Step 5 - Execute Prediction #3

```json
{
  "amount": 890,
  "hour": 2,
  "num_tx_past_day": 3
}
```

Click **Execute**.

Expected response:

```json
{
  "is_fraud": 0
}
```

*(The exact prediction depends on the trained model. The important requirement is that an `is_fraud` field with value `0` or `1` is returned.)*

# Verification

## Verify Swagger UI

Open:

```text
http://localhost:8085/docs
```

The interactive API documentation should load successfully.

## Verify Prediction History

Run:

```bash
curl http://localhost:8085/last-predictions
```

<img width="1462" height="542" alt="2 1" src="https://github.com/user-attachments/assets/15dfbc52-6b22-4d9b-9239-718efa001576" />

Expected output:

```json
{
  "count": 3,
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
    },
    {
      "amount": 890,
      "hour": 2,
      "num_tx_past_day": 3,
      "is_fraud": 0
    }
  ]
}
```

<img width="1465" height="617" alt="2 2" src="https://github.com/user-attachments/assets/9a9d62ea-9f6b-4c7a-b667-36966679fb59" />

The important checks are:

* `count >= 3`
* Three distinct input combinations
* Every prediction includes an `is_fraud` value (`0` or `1`)

# Fun Message 😄

*"With FastAPI's Swagger UI, your API becomes its own playground—no extra tools needed. Just click **Try it out**, send a request, and watch your model come to life! ⚡🤖"*
