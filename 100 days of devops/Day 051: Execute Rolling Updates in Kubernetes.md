# Instructions
We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest changes and those changes need be deployed. The Nautilus DevOps team has created an image `nginx:1.19` with the latest changes.

Perform a rolling update for this application and incorporate `nginx:1.19` image. The deployment name is `nginx-deployment`

Make sure all pods are up and running after the update.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


# Solution
`kubectl get all`

`kubectl describe deploy nginx-deployment`

`kubectl edit deployment nginx-deployment`  change the image

`kubectl describe deploy nginx-deployment`

```YAML
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  11m    deployment-controller  Scaled up replica set nginx-deployment-989f57c54 to 3
  Normal  ScalingReplicaSet  5m     deployment-controller  Scaled up replica set nginx-deployment-dc49f85cc to 1
  Normal  ScalingReplicaSet  4m49s  deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 2 from 3
  Normal  ScalingReplicaSet  4m49s  deployment-controller  Scaled up replica set nginx-deployment-dc49f85cc to 2 from 1
  Normal  ScalingReplicaSet  4m47s  deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 1 from 2
  Normal  ScalingReplicaSet  4m47s  deployment-controller  Scaled up replica set nginx-deployment-dc49f85cc to 3 from 2
  Normal  ScalingReplicaSet  4m45s  deployment-controller  Scaled down replica set nginx-deployment-989f57c54 to 0 from 1
```

Verify that the rolling update is in progress by checking the status of the deployment: `kubectl rollout status deployment/nginx-deployment`
If the rollout has been successfully completed, you'll see output similar to the following:

`kubectl get pods`
