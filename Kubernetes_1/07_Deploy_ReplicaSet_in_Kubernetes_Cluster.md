# Task

The Nautilus DevOps team is gearing up to deploy applications on a Kubernetes cluster for migration purposes. A team member has been tasked with creating a ReplicaSet outlined below:





Create a ReplicaSet using httpd image with latest tag (ensure to specify as httpd:latest) and name it httpd-replicaset.



Apply labels: app as httpd_app, type as front-end.



Name the container httpd-container. Ensure the replica count is 4.



Note: The kubectl utility on jump_host is set up to interact with the Kubernetes cluster.

# Kubernetes ReplicaSet Creation for httpd

This document outlines the process for creating a Kubernetes ReplicaSet to run an `httpd` web server application.

-----

## 🎯 Objective

The goal is to create a Kubernetes ReplicaSet with the following specifications:

  - **Name:** `httpd-replicaset`
  - **Image:** `httpd:latest`
  - **Replicas:** 4
  - **Labels:**
      - `app: httpd_app`
      - `type: front-end`
  - **Container Name:** `httpd-container`

-----

## 📄 Solution: The YAML Manifest

The core of the solution is creating a YAML manifest file that defines the ReplicaSet.

1.  **Create the YAML File**
    On the `jump_host`, create a file named `httpd-replicaset.yaml`.

2.  **Add the following content:**

    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: httpd-replicaset
      labels:
        app: httpd_app
        type: front-end
    spec:
      replicas: 4
      selector:
        matchLabels:
          app: httpd_app
          type: front-end
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

-----

## 🚀 Applying and Verifying the ReplicaSet

1.  **Apply the Manifest**
    Use `kubectl` to create the ReplicaSet in the cluster.

    ```bash
    kubectl apply -f httpd-replicaset.yaml
    ```

    You should see the confirmation message: `replicaset.apps/httpd-replicaset created`.

2.  **Verify the Creation**
    Check that the ReplicaSet and its pods are running correctly.

    ```bash
    # Check the ReplicaSet status
    kubectl get replicaset httpd-replicaset

    # Check the running pods
    kubectl get pods
    ```

    The output should show 4 desired, current, and ready replicas, along with four running pods.
