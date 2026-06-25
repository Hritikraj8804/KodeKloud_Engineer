# Instruction

The xFusionCorp Industries ML Platform team wanted to eliminate hardcoded credentials from service startup scripts by integrating **HashiCorp Vault**.

A development Vault server was already running, while an MLflow startup wrapper continuously polled Vault every 5 seconds for the secret:

```text
secret/mlflow
```

containing:

```text
admin_password
```

MLflow would only start after this secret became available.

The objective was to:

* Enable the KV Version 2 secrets engine
* Create the required secret
* Allow the MLflow wrapper to automatically start MLflow
* Verify both Vault and MLflow were working correctly

# Why Vault?

Instead of storing secrets like:

* MLflow admin password
* PostgreSQL password
* SeaweedFS access keys

inside scripts or Git repositories, Vault securely stores and serves them at runtime.

# Solution Steps

## Step 1 - Export Vault Address

```bash
export VAULT_ADDR=http://127.0.0.1:8200
```

## Step 2 - Read the Root Token

The lab already provides the development root token.

```bash
cat /root/code/vault-token
```

Example:

```text
hvs.xxxxxxxxxxxxxxxxxxxxx
```

## Step 3 - Login to Vault

```bash
vault login
```

Paste the token.

Or login directly:

```bash
vault login $(cat /root/code/vault-token)
```

## Step 4 - Enable KV Version 2

Enable the secrets engine at:

```text
secret/
```

<img width="1825" height="537" alt="screte engines" src="https://github.com/user-attachments/assets/1b00c4cf-1cc8-4568-9392-8215f41c7b9e" />

```bash
vault secrets enable -path=secret kv-v2
```

Expected:

```text
Success! Enabled the kv secrets engine at: secret/
```

<img width="1367" height="687" alt="enable screte engine" src="https://github.com/user-attachments/assets/0d17dfc5-cc14-4aa5-a4e4-968364b6b3b7" />

## Step 5 - Store the MLflow Password

Create the required secret:

```bash
vault kv put secret/mlflow \
admin_password=admin123
```

Any non-empty password satisfies the lab requirement.

Expected:

```text
Success! Data written to: secret/mlflow
```

<img width="952" height="422" alt="mlflow screte" src="https://github.com/user-attachments/assets/3a89ff5f-485c-4ca7-aab4-9639559f0661" />

## Step 6 - Wait for the Wrapper

The startup wrapper polls Vault every:

```text
5 seconds
```

After detecting:

```text
secret/mlflow
```

it automatically starts:

```text
MLflow Server
```

on port:

```text
5000
```

# Verification

## Verify KV Engine

```bash
vault secrets list -detailed
```

Expected:

```text
Path: secret/
Type: kv
Version: 2
```

## Verify Secret

```bash
vault kv get secret/mlflow
```

Expected:

```text
====== Data ======

admin_password
admin123
```

## Verify Using API

```bash
curl \
-H "X-Vault-Token: $(cat /root/code/vault-token)" \
http://127.0.0.1:8200/v1/secret/data/mlflow
```

Expected JSON:

```json
{
  "data": {
    "data": {
      "admin_password": "admin123"
    }
  }
}
```

<img width="742" height="232" alt="cli verify" src="https://github.com/user-attachments/assets/e0b126ba-8b6a-4025-9327-b316a39500bb" />

## Verify MLflow Started

```bash
curl http://localhost:5000/
```

Expected:

```text
HTTP/1.1 200 OK
```

or

```html
<!DOCTYPE html>
...
```

<img width="657" height="330" alt="mlflow run" src="https://github.com/user-attachments/assets/a6defde5-5eae-4cb0-8e23-c9c90c1092e6" />

## Verify in Browser

Open:

```text
Vault UI
```

Login using:

```bash
cat /root/code/vault-token
```

Paste the token.

Navigate to:

```text
Secrets
```

Expected:

```text
secret/

└── mlflow

      admin_password
```

Open:

```text
MLflow UI
```

Expected:

```text
MLflow Dashboard
```

loads successfully.


# Fun Message 😄

*"Hardcoded passwords are like leaving your house key under the doormat. Vault hands the key over only when the right service knocks on the door! 🔐🚪"*
