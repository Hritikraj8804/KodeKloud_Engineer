# Instruction

The xFusionCorp Industries ML Platform team serves multiple ML models behind a single **Nginx reverse proxy**.

Initially, only the **Fraud Detection** and **Churn Prediction** services were configured. A third service, **Recommend**, already existed on disk but was not integrated into the deployment.

The objective was to:

* Add the **recommend** service to `docker-compose.yml`
* Configure an **Nginx upstream** for the recommend service
* Add a **/recommend/** route
* Deploy all services using Docker Compose
* Verify that all three APIs were accessible through Nginx

# Solution

## Updated `docker-compose.yml`

Add the following service:

```yaml
recommend:
  build: ./recommend
  container_name: mm-recommend
```

Also update the nginx dependencies:

```yaml
depends_on:
  - fraud
  - churn
  - recommend
```

## Updated `nginx.conf`

### Add Recommend Upstream

```nginx
upstream recommend_backend {
    server recommend:5000;
}
```

### Add Recommend Route

```nginx
location /recommend/ {
    proxy_pass http://recommend_backend/;
}
```

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/serving/multi-model
```

## Step 2 - Edit Docker Compose

```bash
vi docker-compose.yml
```

Add the `recommend` service and include it under `depends_on`.

## Step 3 - Edit Nginx Configuration

```bash
vi nginx.conf
```

Add:

* `recommend_backend`
* `/recommend/` location block

## Step 4 - Start the Stack

```bash
docker compose up -d
```

Expected output:

```text
Creating mm-fraud ...
Creating mm-churn ...
Creating mm-recommend ...
Creating mm-nginx ...
```

<img width="690" height="162" alt="compose up" src="https://github.com/user-attachments/assets/1b891d14-d36f-4634-bfa8-2d3d00091fb5" />

# Verification

## Verify Running Containers

```bash
docker compose ps
```

Expected:

```text
NAME             STATUS
mm-fraud         Up
mm-churn         Up
mm-recommend     Up
mm-nginx         Up
```

<img width="1017" height="152" alt="ps" src="https://github.com/user-attachments/assets/c601622c-1323-43a7-a831-1a139d71a3e9" />

## Verify Fraud Service

```bash
curl -X POST http://localhost:8085/fraud/predict \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
```

Expected:

```json
{
  "service":"fraud",
  "is_fraud":1
}
```

<img width="827" height="91" alt="fraud" src="https://github.com/user-attachments/assets/e7d6c5bd-b3ba-4d8a-94c0-71667cbbf27c" />

## Verify Churn Service

```bash
curl -X POST http://localhost:8085/churn/predict \
-H "Content-Type: application/json" \
-d '{"customer_age":42}'
```

Expected:

```json
{
  "service":"churn",
  "churn_risk":0
}
```

<img width="807" height="87" alt="churn" src="https://github.com/user-attachments/assets/352e8015-998f-45d7-9bb2-34693a7ba90e" />

## Verify Recommend Service

```bash
curl -X POST http://localhost:8085/recommend/predict \
-H "Content-Type: application/json" \
-d '{"customer_id":101}'
```

Expected:

```json
{
  "service":"recommend",
  "items":[
    "item1",
    "item2",
    "item3"
  ]
}
```

<img width="807" height="92" alt="recommend" src="https://github.com/user-attachments/assets/0ec88174-e26d-48cf-8f66-57405db48596" />

# Fun Message 😄

*"Why expose three different ports when one smart Nginx gateway can route traffic to every model? One endpoint, many intelligent services! 🚀🌐"*
