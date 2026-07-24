# Instruction

The xFusionCorp Industries ML Platform team uses a **champion/challenger deployment strategy** to safely promote retrained models.

Current model registry state:

| Version | Alias        | F1 Score |
| ------- | ------------ | -------: |
| v1      | `production` | **0.71** |
| v2      | *(none)*     | **0.82** |

Your task is to implement the promotion gate in `promote.py` so that:

* Read the current **production** model (champion)
* Read the challenger (v2)
* Compare their **F1 scores**
* Promote **only if** the challenger has a strictly higher F1 score
* Otherwise, leave the production alias unchanged

# Problem Analysis 🔍

MLflow 3.x no longer uses model stages (`Production`, `Staging`).

Instead, deployments use **aliases**:

```text
models:/fraud-detector@production
```

The alias is simply a pointer.

Current state:

```text
production
     │
     ▼
 Version 1
 F1 = 0.71
```

After successful evaluation:

```text
production
     │
     ▼
 Version 2
 F1 = 0.82
```

The promotion gate prevents blindly replacing a good production model with a worse retrained one.

# Solution

## Complete `promote.py`

Replace the `TODO` block with:

```python
def main() -> None:
    # Current production model (champion)
    champion = client.get_model_version_by_alias(MODEL, PROD_ALIAS)
    champion_version = champion.version
    champion_f1 = f1_of(champion_version)

    # Challenger model
    challenger_f1 = f1_of(CHALLENGER_VERSION)

    print(
        f"Champion v{champion_version}: F1={champion_f1:.2f}\n"
        f"Challenger v{CHALLENGER_VERSION}: F1={challenger_f1:.2f}"
    )

    if challenger_f1 > champion_f1:
        client.set_registered_model_alias(
            MODEL,
            PROD_ALIAS,
            CHALLENGER_VERSION,
        )
        print(f"Promoted challenger v{CHALLENGER_VERSION} to production.")
    else:
        print("Challenger rejected. Production alias unchanged.")
```

# Solution Steps

## Step 1

Open the project.

```bash
cd /root/code/monitoring
```

## Step 2

Edit:

```bash
vi promote.py
```

Replace the TODO with the solution above.

## Step 3

Run:

```bash
python3 promote.py
```

<img width="737" height="87" alt="exec" src="https://github.com/user-attachments/assets/638ca812-f13c-48c5-b4e9-3f1409b7db34" />

# Workflow

```text
          Retraining Pipeline
                  │
                  ▼
        Register Challenger (v2)
                  │
                  ▼
         Read Champion Alias
                  │
                  ▼
      Compare F1 Scores
                  │
        ┌─────────┴─────────┐
        │                   │
 Challenger Better?      Challenger Worse?
        │                   │
       Yes                  No
        │                   │
        ▼                   ▼
 Repoint production     Keep Alias
       Alias            Unchanged
```

# Fun Message 😄

*"Retraining creates possibilities—but evaluation creates confidence. A production alias should follow the best model, not simply the newest one! 🏆🤖"*
