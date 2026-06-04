## Instruction

The xFusionCorp Industries Data Science team had accumulated **10 experiment runs** inside the `fraud-detection` MLflow experiment.

The goal was to review the runs using the MLflow UI and classify them based on their **F1 Score** performance.

The task involved:

* Identifying the best-performing model candidate
* Shortlisting it for further review
* Rejecting clearly under-performing runs
* Leaving all other runs unchanged

# Solution

## ✅ Step 1 - Open MLflow UI

Click:

```text
MLflow UI
```

Open:

```text
fraud-detection
```

experiment.


## Step 2 - Sort Runs by F1 Score

In the runs table:

* Locate the **f1_score** column
* Sort descending

Example:

| Run   | F1 Score |
| ----- | -------- |
| Run A | 0.92     |
| Run B | 0.89     |
| Run C | 0.87     |
| Run D | 0.84     |
| Run E | 0.80     |
| Run F | 0.73     |
| Run G | 0.70     |

*(Your values will vary.)*

<img width="1538" height="563" alt="runs" src="https://github.com/user-attachments/assets/303e2d39-dda2-4395-bd7f-4626ec5ab80c" />


## Step 3 - Tag the Best Candidate

Identify:

```text
Highest F1 Score
AND
F1 > 0.85
```

Open that run.

Add tag:

```text
review-status = shortlisted
```

Only this single run should have the shortlist tag.

---

## Step 4 - Tag Rejected Runs

For every run where:

```text
f1_score < 0.75
```

Open the run and add:

```text
review-status = rejected
```

Repeat for all under-performing runs.


## Step 5 - Verify Remaining Runs

Ensure runs with:

```text
0.75 ≤ f1_score ≤ 0.85
```

have:

```text
No review-status tag
```

Also ensure:

```text
Second-best run above 0.85
```

has:

```text
No review-status tag
```


# Example Classification

Example only (actual scores will differ):

| Run   | F1 Score | Tag         |
| ----- | -------- | ----------- |
| Run A | 0.92     | shortlisted |
| Run B | 0.89     | none        |
| Run C | 0.87     | none        |
| Run D | 0.84     | none        |
| Run E | 0.80     | none        |
| Run F | 0.73     | rejected    |
| Run G | 0.70     | rejected    |

<img width="1638" height="708" alt="tags" src="https://github.com/user-attachments/assets/ff72196d-f594-494d-aff1-a6c48a16e6d7" />

# Fun Message 😄

*"Not every model deserves deployment—some deserve a promotion, and some deserve a polite rejection email 😄📊"*
