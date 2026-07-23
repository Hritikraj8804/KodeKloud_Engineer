# Instruction

The xFusionCorp Industries ML Platform team wants all **high-severity alerts** to automatically notify the on-call system through a webhook.

The monitoring stack already includes:

* ✅ Grafana
* ✅ Prometheus datasource
* ✅ Existing alert rules
* ✅ Webhook sink service available at:

```text
http://webhook-sink:5000/hook
```

Your task is to configure:

1. A **Webhook Contact Point**
2. A **Notification Policy** that routes alerts with:

```text
severity = high
```

to that webhook.

# Solution Steps

## Step 1

Open:

```text
http://localhost:3000
```

Login:

```text
admin

grafana2026
```

## Step 2

Navigate:

```text
Alerts

↓

Contact Points

↓

New Contact Point
```

Fill in:

| Field | Value                           |
| ----- | ------------------------------- |
| Name  | High Severity Webhook           |
| Type  | Webhook                         |
| URL   | `http://webhook-sink:5000/hook` |

Save.

<img width="1187" height="881" alt="webhook form" src="https://github.com/user-attachments/assets/4a62b304-81d6-4f57-a344-2dae035ca398" />

## Step 3

Navigate:

```text
Alerts

↓

Notification Policies
```

Create a new policy.

Matcher:

```text
severity = high
```

Receiver:

```text
High Severity Webhook
```

Save.

<img width="902" height="811" alt="add route" src="https://github.com/user-attachments/assets/ad6087af-d185-4388-a85f-8c47c8399f5d" />

# Verification

## Verify Contact Point

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/v1/provisioning/contact-points
```

Expected:

```json
[
  {
    "name": "High Severity Webhook",
    "type": "webhook",
    "settings": {
      "url": "http://webhook-sink:5000/hook"
    }
  }
]
```

<img width="722" height="157" alt="contact point" src="https://github.com/user-attachments/assets/de78549d-f662-40bd-b277-4909af3b2986" />

## Verify Notification Policy

```bash
curl -u admin:grafana2026 \
http://localhost:3000/api/v1/provisioning/policies
```

Expected policy tree contains:

```text
severity = high
```

and receiver:

```text
High Severity Webhook
```

<img width="722" height="147" alt="alert" src="https://github.com/user-attachments/assets/bbca55e3-6c38-42ae-a748-3c2653654ab0" />

# Workflow

```text
           Alert Rule
                │
                ▼
      Labels: severity=high
                │
                ▼
      Notification Policy
      Match: severity=high
                │
                ▼
      Contact Point (Webhook)
                │
                ▼
http://webhook-sink:5000/hook
                │
                ▼
      On-call Notification
```

# Fun Message 😄

*"Detecting an issue is only half the job—routing it to the right people at the right time is what turns monitoring into reliable incident response! 🚨📡"*
