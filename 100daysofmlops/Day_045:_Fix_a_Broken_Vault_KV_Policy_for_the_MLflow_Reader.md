# Instruction

The xFusionCorp Industries ML Platform team improved the security model introduced in the previous lab.

Instead of allowing the MLflow startup wrapper to authenticate using the **Vault root token**, the wrapper now uses a dedicated **mlflow-reader** token with a limited-access policy.

Unfortunately, the policy was misconfigured, causing every request to Vault to fail with:

```text
permission denied
```

As a result, MLflow never received its password and failed to start.

The objective was to:

* Correct the existing `mlflow-reader` policy
* Allow only read access to `secret/data/mlflow`
* Keep the policy name unchanged
* Verify the limited token can read the secret
* Confirm MLflow automatically starts

# Solution Steps

## Step 1 - Login to Vault UI

Open:

```text
Vault UI
```

Use the **root token** (not the service token):

```bash
cat /root/code/vault-root-token
```

Paste the token into the login screen.

## Step 2 - Open Policies

Navigate:

```text
Access
    ↓
Policies
```

Open the existing policy:

```text
mlflow-reader
```

<img width="1845" height="482" alt="policies" src="https://github.com/user-attachments/assets/ec9c244a-4e1b-4ddd-bdde-455b085767db" />

**Do not delete or rename it.**

## Step 3 - Correct the Policy

Replace the incorrect rule with:

```hcl
path "secret/data/mlflow" {
  capabilities = ["read"]
}
```

This grants the narrow token permission to read exactly one secret.

Save the policy.

# Correct Policy

```hcl
path "secret/data/mlflow" {
  capabilities = ["read"]
}
```

<img width="856" height="417" alt="policies update" src="https://github.com/user-attachments/assets/62ebec69-6093-406d-99b1-fd7bc3cef420" />

# Wait for the Wrapper

The MLflow wrapper polls Vault every:

```text
5 seconds
```

After saving the policy:

```text
permission denied
```

changes to:

```text
200 OK
```

The wrapper automatically launches:

```text
MLflow Server
```

on:

```text
localhost:5000
```

<img width="1152" height="412" alt="mlflow ui load" src="https://github.com/user-attachments/assets/b8a9b965-8772-4ad4-819f-89f9a6249566" />

No manual restart is required.

# Verification

## Verify Policy

Run:

```bash
export VAULT_ADDR=http://127.0.0.1:8200

export VAULT_TOKEN=$(cat /root/code/vault-root-token)

vault policy read mlflow-reader
```

Expected:

```hcl
path "secret/data/mlflow" {
  capabilities = ["read"]
}
```

<img width="700" height="300" alt="cli verify" src="https://github.com/user-attachments/assets/7775b597-de8e-4c7a-985c-1b3dbdbb9211" />

## Verify Narrow Token

Switch to the service token:

```bash
export VAULT_TOKEN=$(cat /root/code/vault-token)
```

Retrieve the secret:

```bash
vault kv get secret/mlflow
```

Expected:

```text
====== Data ======

admin_password
********
```

No:

```text
permission denied
```

should appear.

## Verify Using HTTP API

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
      "admin_password": "..."
    }
  }
}
```

## Verify MLflow

```bash
curl http://localhost:5000/
```

Expected:

```text
HTTP/1.1 200 OK
```

or the MLflow HTML page.

## Verify UI

Open:

```text
MLflow UI
```

The dashboard should load successfully.

# Fun Message 😄

*"A good security policy doesn't ask 'Who are you?' It asks 'Exactly what are you allowed to touch?' 🔐✨"*
