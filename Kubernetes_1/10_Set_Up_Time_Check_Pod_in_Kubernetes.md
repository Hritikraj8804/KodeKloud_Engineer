# Task

The Nautilus DevOps team needs a time check pod created in a specific Kubernetes namespace for logging purposes. Initially, it's for testing, but it may be integrated into an existing cluster later. Here's what's required:



Create a pod called time-check in the nautilus namespace. The pod should contain a container named time-check, utilizing the busybox image with the latest tag (specify as busybox:latest).



Create a config map named time-config with the data TIME_FREQ=12 in the same namespace.



Configure the time-check container to execute the command: while true; do date; sleep $TIME_FREQ;done. Ensure the result is written /opt/sysops/time/time-check.log. Also, add an environmental variable TIME_FREQ in the container, fetching its value from the config map TIME_FREQ key.

Create a volume log-volume and mount it at /opt/sysops/time within the container.

Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

# Solution

### ⚙️ 1. Create Namespace and ConfigMap

First, create the `nautilus` namespace and the `time-config` ConfigMap. These can be done with simple `kubectl` commands.

1.  **Create the Namespace**:
    ```bash
    kubectl create namespace nautilus
    ```
2.  **Create the ConfigMap**:
    This command creates the `time-config` ConfigMap with the required `TIME_FREQ=12` data in the `nautilus` namespace.
    ```bash
    kubectl create configmap time-config --from-literal=TIME_FREQ=12 -n nautilus
    ```

-----

### 📄 2. Create the Pod Manifest

Next, create a YAML file for the `time-check` pod. This is the best way to define a pod with volumes, environment variables, and custom commands.

1.  **Create the YAML file** on the `jump_host`:

    ```bash
    vi time-check-pod.yaml
    ```

2.  **Add the following manifest content**:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: time-check
      namespace: nautilus
    spec:
      volumes:
        - name: log-volume
          emptyDir: {}
      containers:
        - name: time-check
          image: busybox:latest
          env:
            - name: TIME_FREQ
              valueFrom:
                configMapKeyRef:
                  name: time-config
                  key: TIME_FREQ
          volumeMounts:
            - name: log-volume
              mountPath: /opt/sysops/time
          command: ["/bin/sh", "-c"]
          args:
            - >
              while true; do
                date >> /opt/sysops/time/time-check.log;
                sleep $TIME_FREQ;
              done
    ```

    Save and exit the editor (`:wq` in `vi`).

-----

### 🚀 3. Apply and Verify

Finally, apply the manifest to create the pod and verify that it's working correctly.

1.  **Apply the Manifest**:

    ```bash
    kubectl apply -f time-check-pod.yaml
    ```

    You should see the output: `pod/time-check created`.

2.  **Verify the Pod Status**:
    Check to see that the `time-check` pod is running in the `nautilus` namespace.

    ```bash
    kubectl get pods -n nautilus
    ```

    The status should be `Running`.

3.  **Verify the Log Output**:
    This is the final check. Use `kubectl exec` to look inside the pod and view the log file being created.

    ```bash
    # Wait about 15-20 seconds for some logs to generate
    kubectl exec time-check -n nautilus -- tail /opt/sysops/time/time-check.log
    ```

    You should see a series of timestamps printed to the console, confirming that the volume mount, environment variable, and command are all working correctly.
