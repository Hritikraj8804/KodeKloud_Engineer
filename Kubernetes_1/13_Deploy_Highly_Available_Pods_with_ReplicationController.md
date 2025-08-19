# Task

The Nautilus DevOps team is establishing a ReplicationController to deploy multiple pods for hosting applications that require a highly available infrastructure. Follow the specifications below to create the ReplicationController:



Create a ReplicationController using the httpd image with latest tag, and name it httpd-replicationcontroller.



Assign labels app as httpd_app, and type as front-end. Ensure the container is named httpd-container and set the replica count to 3.



All pods should be running state post-deployment.



Note: The kubectl utility on jump_host is configured to operate with the Kubernetes cluster.

# Solution


### \#\# 📄 1. Create the ReplicationController Manifest

First, create a YAML file on the `jump_host`. You can name it `httpd-rc.yaml`.

```bash
vi httpd-rc.yaml
```

Copy and paste the following manifest into the file. This configuration defines the ReplicationController with the required image, labels, container name, and replica count.

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: httpd-replicationcontroller
  labels:
    app: httpd_app
    type: front-end
spec:
  replicas: 3
  selector:
    app: httpd_app
  template:
    metadata:
      labels:
        app: httpd_app
        type: front-end
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest
```

Save and exit the editor (`:wq` in `vi`).

-----

### \#\# 🚀 2. Apply the Manifest

Use `kubectl apply` to create the ReplicationController in the Kubernetes cluster.

```bash
kubectl apply -f httpd-rc.yaml
```

You should see the output `replicationcontroller/httpd-replicationcontroller created`.

-----

### \#\# ✅ 3. Verify the ReplicationController

Finally, verify that the ReplicationController and its pods were created successfully.

1.  **Check the ReplicationController**:
    This command shows the desired and current number of replicas.

    ```bash
    kubectl get rc httpd-replicationcontroller
    ```

    The output should show `DESIRED: 3`, `CURRENT: 3`, and `READY: 3`.

2.  **Check the Pods**:
    This command lists the individual pods managed by the controller.

    ```bash
    kubectl get pods
    ```

    You should see three running pods with names like `httpd-replicationcontroller-xxxxx`.
