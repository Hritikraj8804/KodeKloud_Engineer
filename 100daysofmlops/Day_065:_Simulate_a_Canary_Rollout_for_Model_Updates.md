# Instruction

The xFusionCorp Industries ML Platform team practices **canary deployments** using a pure-Python simulator before adopting the same rollout strategy in **Argo Rollouts**.

The provided `canary_deploy.py` scaffold already simulated requests and monitored the candidate model's error rate, but the actual rollout policy was left as **TODOs**.

The objective was to:

* Set an appropriate rollback threshold
* Implement the three-phase canary traffic ramp
* Promote the rollout when the candidate remains healthy
* Ensure the simulator ends with **OUTCOME: PROMOTED**


# Solution

## Final Changes in `canary_deploy.py`

```python
ROLLBACK_THRESHOLD = 0.05
```

<img width="366" height="62" alt="rollback" src="https://github.com/user-attachments/assets/c826345c-5190-4469-b8f7-28f07bc606cd" />

### Update `promote()`

```python
def promote(self) -> tuple[float, float]:
    self.phase += 1

    if self.phase == 1:
        self.v1_weight = 0.95
        self.v2_weight = 0.05
    elif self.phase == 2:
        self.v1_weight = 0.70
        self.v2_weight = 0.30
    elif self.phase == 3:
        self.v1_weight = 0.00
        self.v2_weight = 1.00

    return self.v1_weight, self.v2_weight
```

<img width="610" height="377" alt="promte" src="https://github.com/user-attachments/assets/952214f5-97b3-4180-afa5-de4a99545c94" />

# Solution Steps

## Step 1 - Navigate to Project

```bash
cd /root/code/serving
```


## Step 2 - Edit the Simulator

```bash
vi canary_deploy.py
```

Implement:

* `ROLLBACK_THRESHOLD = 0.05`
* Three-phase traffic ramp inside `promote()`


## Step 3 - Run the Simulator

```bash
python3 canary_deploy.py
```

Expected output:

```text
Phase 1: v1=95% v2=5%
  v1_requests=95 v2_requests=5 v2_error_rate=0.00%

Phase 2: v1=70% v2=30%
  v1_requests=69 v2_requests=31 v2_error_rate=3.23%

Phase 3: v1=0% v2=100%
  v1_requests=0 v2_requests=100 v2_error_rate=2.00%

OUTCOME: PROMOTED
Total requests: 300
```

(The exact request counts and error percentages may vary slightly because routing is randomized.)

<img width="462" height="252" alt="run" src="https://github.com/user-attachments/assets/20f8a0ea-ea45-4f87-9257-3aaaff99a131" />

# Verification

## Verify Promotion

```bash
python3 canary_deploy.py | tail -2
```

Expected:

```text
OUTCOME: PROMOTED
Total requests: 300
```




## Verify Final Promotion

Expected:

```text
OUTCOME: PROMOTED
```

# Fun Message 😄

*"Canary deployments let a new model earn users' trust one request at a time—if it behaves well, it gets promoted; if not, it quietly flies back to safety! 🐤🚀"*
