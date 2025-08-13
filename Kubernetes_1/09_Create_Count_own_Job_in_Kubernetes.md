# Task

The Nautilus DevOps team is crafting jobs in the Kubernetes cluster. While they're developing actual scripts/commands, they're currently setting up templates and testing jobs with dummy commands. Please create a job template as per details given below:





Create a job named countdown-datacenter.



The spec template should be named countdown-datacenter (under metadata), and the container should be named container-countdown-datacenter



Utilize image fedora with latest tag (ensure to specify as fedora:latest), and set the restart policy to Never.



Execute the command sleep 5



Note: The kubectl utility on jump_host is set up to operate with the Kubernetes cluster.


# Soludtion 

### 📄 1. Create the Job Manifest

First, create a YAML file on the `jump_host`. You can name it `countdown-job.yaml`.

```bash
vi countdown-job.yaml
```

Copy and paste the following manifest into the file. This configuration defines the Job with the specified template name, container details, image, command, and restart policy.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: countdown-datacenter
spec:
  template:
    metadata:
      name: countdown-datacenter
    spec:
      containers:
      - name: container-countdown-datacenter
        image: fedora:latest
        command: ["/bin/sh", "-c", "sleep 5"]
      restartPolicy: Never
```

Save and exit the editor (`:wq` in `vi`).

-----

###  🚀 2. Apply the Manifest

Use `kubectl apply` to create the Job in the Kubernetes cluster.

```bash
kubectl apply -f countdown-job.yaml
```

You should see the output: `job.batch/countdown-datacenter created`.

-----

###  ✅ 3. Verify the Job

Finally, verify that the Job ran and completed successfully.

1.  **Check the Job Status**:
    This command shows the status of the Job. After a few seconds, `COMPLETIONS` should show `1/1`.

    ```bash
    kubectl get job countdown-datacenter
    ```

2.  **Check the Pod**:
    This command lists the pod created by the Job. Its status should change from `Running` to `Completed`.

    ```bash
    kubectl get pods
    ```
