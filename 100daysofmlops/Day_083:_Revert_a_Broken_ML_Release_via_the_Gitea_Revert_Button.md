# Instruction

The xFusionCorp Industries ML Platform team accidentally merged a Pull Request titled **"Add speculative hashing scaffold"**, which introduced a lint regression and caused the **main** branch CI to remain red.

According to the team's rollback policy:

* ❌ No command-line `git revert`
* ❌ No force-push
* ❌ No direct edits on `main`

Instead, the rollback must be performed entirely through **Gitea's built-in Revert button**, creating a new reviewable Pull Request that restores the previous state while preserving the complete audit trail.

# Solution

## Perform the Revert Using the Gitea UI

No local Git commands are required.

# Solution Steps

## Step 1 - Open Gitea

Open:

```text
http://localhost:3000
```

Login:

```text
Username:
gitea-admin

Password:
gitea2026
```

## Step 2 - Open the Repository

Navigate to:

```text
fraud-detector
```

## Step 3 - Locate the Original PR

Go to:

```text
Pull Requests

↓

Closed
```

Open:

```text
Add speculative hashing scaffold
```

<img width="1236" height="840" alt="failed pr" src="https://github.com/user-attachments/assets/15667add-098e-4351-9897-85c6608e30e8" />

## Step 4 - Click **Revert**

On the merged PR page click:

```text
Revert
```

Gitea automatically creates:

* a new revert branch
* a new Pull Request

The new PR title will resemble:

```text
Revert "Add speculative hashing scaffold"
```

<img width="867" height="742" alt="revert" src="https://github.com/user-attachments/assets/d7e70251-38a0-47df-bdd6-b8df8702453c" />

## Step 5 - Open the Revert PR

Review the generated changes.

No manual code edits are needed.

## Step 6 - Wait for CI

The revert PR automatically triggers:

```text
✔ lint

✔ test

✔ report
```

(or whatever CI workflow the repository defines)

Wait until every check is green.

<img width="1237" height="825" alt="success pr" src="https://github.com/user-attachments/assets/06368a96-6b60-46ef-9851-6aa0a5141d11" />

## Step 7 - Merge the Revert PR

Once all checks succeed:

Click

```text
Merge Pull Request
```

The revert commit is now added to **main**.

## Step 8 - Verify Main

Open:

```text
Actions
```

Latest run:

```text
✔ Success
```

<img width="1257" height="752" alt="merge" src="https://github.com/user-attachments/assets/3512563c-420e-46dd-9b46-bf6e5681a2f3" />

# Workflow

```text
Broken Merge
      │
      ▼
Open Closed PR
      │
      ▼
Click Revert
      │
      ▼
New Revert Branch
      │
      ▼
Revert Pull Request
      │
      ▼
CI Validation
      │
      ▼
Merge Revert PR
      │
      ▼
main Restored
      │
      ▼
Green CI
```

# Fun Message 😄

*"Good engineers don't erase history—they document it. A well-reviewed revert is often the fastest and safest path back to a healthy production branch! 🔄🚦"*
