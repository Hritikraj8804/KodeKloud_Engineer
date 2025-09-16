# Task

An application deployed on the Kubernetes cluster requires an update with new features developed by the Nautilus application development team. The existing setup includes a deployment named nginx-deployment and a service named nginx-service. Below are the necessary changes to be implemented without deleting the deployment and service:



1.) Modify the service nodeport from 30008 to 32165

2.) Change the replicas count from 1 to 5

3.) Update the image from nginx:1.19 to nginx:latest

Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

# Solution


You can accomplish these updates using specific `kubectl` commands to modify the live resources without deleting them.

-----

### \#\# 1. Modify the Service NodePort

Use the `kubectl edit` command to interactively change the service's port.

1.  **Open the service configuration for editing:**
    ```bash
    kubectl edit service nginx-service
    ```
2.  This will open the service's YAML manifest in your default text editor. Find the `nodePort` field and change its value from `30008` to `32165`.
3.  Save and close the file to apply the change.

-----

### \#\# 2. Change the Replica Count

The `kubectl scale` command is the most direct way to change the number of replicas.

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

This command will immediately start scaling the deployment to 5 pods.

-----

### \#\# 3. Update the Container Image

Use the `kubectl set image` command to perform a rolling update of the container image.

```bash
kubectl set image deployment nginx-deployment nginx=nginx:latest
```

**Note:** This command assumes the container in your deployment is named `nginx`. If you're unsure, you can find the container name by running `kubectl describe deployment nginx-deployment` first.

-----

### \#\# ✅ 4. Verification

After performing the updates, you can verify the changes with the following commands:

  * **Check the deployment's replicas and image:**

    ```bash
    kubectl get deployment nginx-deployment
    ```

    The output should show `5/5` under the `READY` column.

  * **Check the service's new port:**

    ```bash
    kubectl get service nginx-service
    ```

    The output will show the new `NodePort` `32165`.
