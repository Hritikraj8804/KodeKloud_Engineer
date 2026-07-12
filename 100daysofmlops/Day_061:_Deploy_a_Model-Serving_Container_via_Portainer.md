# Instruction

The xFusionCorp Industries ML Platform team manages every model-serving container through **Portainer**.

A pre-built Docker image named **fraud-detector:v1** was already available. The objective was to deploy this image using the **Portainer UI**, bind-mount the serving directory so the container could access `app.py` and `model.pkl`, expose the API on **host port 8085**, and verify that both the **/health** and **/predict** endpoints worked correctly.

# Problem Analysis 🔍

The Docker image was already built, but no container had been deployed.

The deployment required the following configuration inside **Portainer**:

* Container Name → `fraud-api`
* Image → `fraud-detector:v1`
* Bind Mount

  * Host: `/root/code/serving`
  * Container: `/app`
* Port Mapping

  * Host Port: `8085`
  * Container Port: `8085`

Without the bind mount, the container would not be able to locate:

* `app.py`
* `model.pkl`

Without the port mapping, the FastAPI service would not be reachable from the host.


# Solution Steps

## Step 1 - Login to Portainer

```
http://localhost:9090
```

## Step 2 - Create Container

Navigate to:

```
Local
    ↓
Containers
    ↓
Add Container
```

<img width="1907" height="790" alt="local" src="https://github.com/user-attachments/assets/6f80b37b-01b3-495b-a44c-ddd536f15c2b" />

Container Name

```
fraud-api
```

Image

```
fraud-detector:v1
```

<img width="1506" height="380" alt="image conf" src="https://github.com/user-attachments/assets/ba7da125-1af4-4f9b-b9e7-fd3a6c605e5a" />

## Step 3 - Configure Port Mapping

```
Host Port      : 8085
Container Port : 8085
```

<img width="925" height="191" alt="port conf" src="https://github.com/user-attachments/assets/ab464e87-ef81-4257-b97b-1a6ce8afaba8" />

## Step 4 - Configure Volume

Choose **Bind**

```
Host

/root/code/serving
```

```
Container

/app
```

<img width="1246" height="386" alt="volu conf" src="https://github.com/user-attachments/assets/309b129a-cdef-4fa5-b4b2-ba9034b36965" />

## Step 5 - Deploy

Click

```
Deploy the container
```

<img width="1527" height="420" alt="deployed" src="https://github.com/user-attachments/assets/32efc49d-fa7e-452d-9a1e-de356e61b262" />

# Verification

## Verify Container is Running

```bash
docker inspect fraud-api
```

## Verify Image

```bash
docker inspect -f '{{.Config.Image}}' fraud-api
```

Expected

```text
fraud-detector:v1
```

<img width="732" height="617" alt="cli veri" src="https://github.com/user-attachments/assets/212d9d3e-ebdc-4d2e-848e-6a733115db9d" />

## Verify Bind Mount

```bash
docker inspect -f '{{range .Mounts}}{{println .Source "->" .Destination}}{{end}}' fraud-api
```

Expected

```text
/root/code/serving -> /app
```

## Verify Health Endpoint

```bash
curl http://localhost:8085/health
```

Expected

```json
{"status":"ok"}
```

## Verify Prediction Endpoint

```bash
curl -X POST http://localhost:8085/predict \
-H "Content-Type: application/json" \
-d '{"amount":3200,"hour":23,"num_tx_past_day":5}'
```

Expected

```json
{"is_fraud":1}
```

*(Depending on the trained model, the value may also be `0`.)*

<img width="702" height="151" alt="curl verif" src="https://github.com/user-attachments/assets/6929e4a8-3379-4c70-b1f4-be45167832b4" />

# Fun Message 😄

*"Portainer turns complex Docker commands into a few clicks—making container deployment as easy as filling out a web form! 🐳✨"*
