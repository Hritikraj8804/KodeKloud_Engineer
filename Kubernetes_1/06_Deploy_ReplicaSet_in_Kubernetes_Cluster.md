# Instructions

Earlier today, the Nautilus DevOps team deployed a new release for an application. However, a customer has reported a bug related to this recent release. Consequently, the team aims to revert to the previous version.



There exists a deployment named nginx-deployment; initiate a rollback to the previous revision.

Note: The kubectl utility on jump_host is configured to interact with the Kubernetes cluster.

# Solution 
### ⏪ Initiate the Rollback

To revert the **`nginx-deployment`** to its previous revision, run the following command from the `jump_host`:

```bash
kubectl rollout undo deployment nginx-deployment
```

This command tells Kubernetes to "undo" the most recent deployment, effectively reverting to the state of the previous revision.

-----

### ✅ Verify the Rollback (Recommended)

After initiating the rollback, it's good practice to verify that it completed successfully.

1.  **Check the Rollout Status**:
    This command will show you the progress of the rollback and confirm when it's finished.

    ```bash
    kubectl rollout status deployment nginx-deployment
    ```

    You should see a message like `deployment "nginx-deployment" successfully rolled out`.

2.  **Inspect the Rollout History**:
    You can also check the deployment history again to see that the previous revision is now the active one.

    ```bash
    kubectl rollout history deployment nginx-deployment
    ```
