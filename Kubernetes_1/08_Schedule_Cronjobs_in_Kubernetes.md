# Task

The Nautilus DevOps team is setting up recurring tasks on different schedules. Currently, they're developing scripts to be executed periodically. To kickstart the process, they're creating cron jobs in the Kubernetes cluster with placeholder commands. Follow the instructions below:





Create a cronjob named xfusion.



Set Its schedule to something like */12 * * * *. You can set any schedule for now.



Name the container cron-xfusion.



Utilize the httpd image with latest tag (specify as httpd:latest).



Execute the dummy command echo Welcome to xfusioncorp!.



Ensure the restart policy is OnFailure.



Note: The kubectl utility on jump_host is configured to work with the kubernetes cluster.

# Solution

### 📄 1. Create the CronJob Manifest

First, create a YAML file on the `jump_host`. You can name it `xfusion-cronjob.yaml`.

```bash
vi xfusion-cronjob.yaml
```

Copy and paste the following manifest into the file. This configuration defines the CronJob with the specified schedule, container details, command, and restart policy.

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: xfusion
spec:
  schedule: "*/12 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: cron-xfusion
            image: httpd:latest
            command: ["/bin/sh", "-c", "echo Welcome to xfusioncorp!"]
          restartPolicy: OnFailure
```

Save and exit the editor (`:wq` in `vi`).

-----

### \#\# 🚀 2. Apply the Manifest

Use `kubectl apply` to create the CronJob in the Kubernetes cluster.

```bash
kubectl apply -f xfusion-cronjob.yaml
```

You should see the output: `cronjob.batch/xfusion created`.

-----

### \#\# ✅ 3. Verify the CronJob

Finally, verify that the CronJob was created successfully.

1.  **Check the CronJob**:
    This command shows that the CronJob is scheduled.

    ```bash
    kubectl get cronjob xfusion
    ```

    The output should show the name `xfusion` with your specified schedule.

2.  **Check for Jobs**:
    Over time, the CronJob will create Job objects based on its schedule. You can watch for them with this command.

    ```bash
    kubectl get jobs --watch
    ```

3.  **Check the Pods**:
    Each Job creates a pod to run the command. You can check the logs of a completed pod to see your echo command's output.

    ```bash
    # First, get the pod name created by a job
    kubectl get pods

    # Then, check its logs (replace <pod-name> with the actual name)
    kubectl logs <pod-name>
    ```

    The log output should be: `Welcome to xfusioncorp!`.
