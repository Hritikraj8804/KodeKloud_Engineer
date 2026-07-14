# Instruction

The xFusionCorp Industries ML Platform team implemented an **asynchronous fraud-detection API** to keep HTTP response times extremely low.

Instead of waiting for the model to finish predicting, the API should:

* Immediately return a **task_id**
* Run the prediction in a **background thread**
* Store the prediction result in **Redis**
* Allow clients to retrieve the result later through a polling endpoint

The scaffold already handled:

* Loading the model
* Starting the background worker
* Connecting to Redis
* Returning a `task_id`

The missing implementation was:

* Store the prediction result in Redis
* Retrieve the stored result from Redis

# Solution

## Final `_run_prediction()` Implementation

```python
def _run_prediction(task_id: str, features) -> None:
    time.sleep(0.3)

    is_fraud = int(MODEL.predict(np.array([features]))[0])

    REDIS.set(
        RESULT_KEY.format(task_id=task_id),
        is_fraud,
        ex=RESULT_TTL_SECONDS,
    )
```

## Final `/result/<task_id>` Endpoint

```python
@app.route("/result/<task_id>")
def result(task_id):
    value = REDIS.get(
        RESULT_KEY.format(task_id=task_id)
    )

    if value is None:
        return jsonify({
            "task_id": task_id,
            "status": "pending"
        }), 200

    return jsonify({
        "task_id": task_id,
        "is_fraud": int(value)
    }), 200
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/serving
```

## Step 2 - Edit Application

```bash
vi async_app.py
```

Implement the Redis storage and retrieval logic.

## Step 3 - Start the Flask Server

```bash
python3 async_app.py &
```

Expected output:

```text
* Running on http://0.0.0.0:8085
```

<img width="745" height="271" alt="exec" src="https://github.com/user-attachments/assets/c5f8f8e2-7d70-426e-ba7c-0c398800a495" />

## Step 4 - Submit an Async Prediction

```bash
curl -X POST http://localhost:8085/predict-async \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
```

Example output:

```json
{
  "task_id":"1d9ce9ef5e5d470d96d0f8d48f26b3cf"
}
```

<img width="687" height="130" alt="test" src="https://github.com/user-attachments/assets/38160120-beb6-4757-97a4-91dbd9e96837" />

Save the returned **task_id**.

## Step 5 - Poll the Result

Immediately after submitting:

```bash
curl http://localhost:8085/result/1d9ce9ef5e5d470d96d0f8d48f26b3cf
```

Possible output while prediction is still running:

```json
{
  "task_id":"1d9ce9ef5e5d470d96d0f8d48f26b3cf",
  "status":"pending"
}
```

<img width="685" height="87" alt="pending" src="https://github.com/user-attachments/assets/f9770bdc-4562-476f-9cc4-b59c69dc4953" />

Wait a second and retry.

Expected:

```json
{
  "task_id":"1d9ce9ef5e5d470d96d0f8d48f26b3cf",
  "is_fraud":1
}
```

<img width="675" height="85" alt="verify" src="https://github.com/user-attachments/assets/9ebb0cc5-f7b3-4932-ba55-593e82409882" />

# Verification

## Verify Health Endpoint

```bash
curl http://localhost:8085/health
```

Expected

```json
{"status":"ok"}
```

## Verify Async Submission

```bash
curl -X POST http://localhost:8085/predict-async \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
```

Expected

```json
{
  "task_id":"xxxxxxxxxxxxxxxx"
}
```

## Verify Result Retrieval

```bash
curl http://localhost:8085/result/<task_id>
```

Expected

```json
{
  "task_id":"xxxxxxxxxxxxxxxx",
  "is_fraud":1
}
```

or

```json
{
  "task_id":"xxxxxxxxxxxxxxxx",
  "is_fraud":0
}
```

## Verify Redis Key

```bash
redis-cli GET result:<task_id>
```

Expected

```text
0
```

or

```text
1
```

## Verify TTL

```bash
redis-cli TTL result:<task_id>
```

Expected

```text
599
```

(or any value counting down from 600)


# Fun Message 😄

*"Fast APIs don't always finish the work immediately—they hand you a ticket, finish the job in the background, and let you come back later for the result! ⚡🔴"*
