# Task

A junior DevOps team member encountered difficulties deploying a stack on the Kubernetes cluster. The pod fails to start, presenting errors. Let's troubleshoot and rectify the issue promptly.



There is a pod named webserver, and the container within it is named nginx-container, its utilizing the nginx:latest image.



Additionally, there's a sidecar container named sidecar-container using the ubuntu:latest image.

Identify and address the issue to ensure the pod is in the running state and the application is accessible.

Note: The kubectl utility on jump_host is configured to interact with the Kubernetes cluster.

# Solution

The issue is that the `sidecar-container` using the `ubuntu:latest` image exits immediately because it has no long-running command. In a pod with multiple containers, all containers must be running for the pod to be in a `Running` state. When the sidecar exits, Kubernetes restarts it, leading to a `CrashLoopBackOff` error.

To fix this, you need to provide a command to the sidecar container to keep it active.

-----

### \#\# 🩺 1. Diagnose the Pod

First, confirm the pod's status and inspect its events to understand the failure.

1.  **Check the Pod Status**:
    You'll likely see a status like `CrashLoopBackOff` or `Error`.
    ```bash
    kubectl get pod webserver
    ```
2.  **Describe the Pod**:
    This is the most important troubleshooting command. It will show you the events at the bottom, which will describe the container starting, stopping, and being restarted.
    ```bash
    kubectl describe pod webserver
    ```

-----

### \#\# 🛠️ 2. Resolution

The solution is to edit the pod's definition to add a command that keeps the `sidecar-container` running indefinitely.

1.  **Get the Pod's YAML and Delete It**:
    First, save the current configuration to a file, then delete the failing pod so you can recreate it from the fixed file.

    ```bash
    kubectl get pod webserver -o yaml > webserver.yaml
    kubectl delete pod webserver
    ```

2.  **Edit the YAML file**:
    Open the `webserver.yaml` file you just created.

    ```bash
    vi webserver.yaml
    ```

    Find the definition for `sidecar-container` and add a `command` and `args` to it, like so:

    **Before:**

    ```yaml
      - image: ubuntu:latest
        name: sidecar-container
    ```

    **After:**

    ```yaml
      - image: ubuntu:latest
        name: sidecar-container
        command: ["/bin/sh", "-c"]
        args: ["while true; do sleep 3600; done"]
    ```

    Save and close the file (`:wq`).

3.  **Re-create the Pod**:
    Apply the corrected manifest to create the pod.

    ```bash
    kubectl apply -f webserver.yaml
    ```

-----

### \#\# ✅ 3. Verification

Finally, check the status of the new pod.

```bash
kubectl get pod webserver
```

The pod's status should now be `Running`, as both the Nginx container and the sidecar container will remain active.
