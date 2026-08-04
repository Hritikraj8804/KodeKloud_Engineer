# Instruction

The xFusionCorp Industries ML Platform team wanted to strengthen the governance of the **fraud-detector** repository after an incident where an administrator force-merged a Pull Request despite failing CI.

The task had two objectives:

* Enhance the CI workflow by adding a **test** job alongside the existing **lint** job.
* Configure **Gitea Branch Protection** so that:

  * All changes must go through Pull Requests.
  * Both **lint** and **test** checks must pass before merging.
  * At least **one approving review** is required.
  * Direct pushes to `main` are blocked.


# Solution Steps

## Step 1 - Navigate to Repository

```bash
cd /root/code/fraud-detector
```

## Step 2 - Edit Workflow

```bash
vi .gitea/workflows/ci.yml
```

Add the **test** job shown above.

## Step 3 - Commit and Push

```bash
git add .gitea/workflows/ci.yml

git commit -m "Add test job to CI"

git push origin main
```

<img width="727" height="286" alt="code push" src="https://github.com/user-attachments/assets/df9bad29-515b-4de5-a87e-25c43127d2cf" />

Wait until both jobs complete successfully.

## Step 4 - Open Gitea

Open:

```text
http://localhost:3000
```

Login

```text
Username:
gitea-admin

Password:
gitea2026
```

## Step 5 - Configure Branch Protection

Navigate to

```text
Repository

↓

Settings

↓

Branches
```

Create or edit the protection rule for:

```text
main
```

Configure:

### Require Pull Request

```text
✔ Enable
```

### Require Status Checks

```text
✔ Enable Status Checks
```

Required contexts:

```text
lint

test
```

### Require Reviews

```text
Required Approvals:

1
```

### Block Direct Pushes

Disable direct pushes by either:

```text
Enable Push

❌ Disabled
```

or

```text
Enable Push Whitelist

✔ Enabled

Whitelist:

(empty)
```

<img width="1647" height="496" alt="branch protection" src="https://github.com/user-attachments/assets/11daecf2-68ff-46ab-8dcf-83d49ddc6947" />

Save the rule.

# Verification

## Verify CI Jobs

Workflow should now contain:

```text
lint

test
```

Both execute successfully ✔

<img width="1085" height="567" alt="jobs" src="https://github.com/user-attachments/assets/6ddfa29a-4abc-46fb-a0b3-dfa5f0c8be2e" />

<img width="727" height="387" alt="verify" src="https://github.com/user-attachments/assets/4a187134-1697-4d63-92e8-c11d160303b0" />

# Workflow

```text
Developer
      │
      ▼
Open Pull Request
      │
      ▼
CI Starts
 ├───────────────┐
 ▼               ▼
Lint          Tests
 └───────────────┘
        │
        ▼
Both Green
        │
        ▼
Reviewer Approval
        │
        ▼
Merge Allowed
        │
        ▼
Protected main
```

# Fun Message 😄

*"The best production incidents are the ones that never happen. Strong CI and branch protection turn risky merges into safe, predictable releases! 🛡️✅"*
