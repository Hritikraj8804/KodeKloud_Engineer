# Instructions
We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest changes and those changes need be deployed. The Nautilus DevOps team has created an image `nginx:1.19` with the latest changes.

Perform a rolling update for this application and incorporate `nginx:1.19` image. The deployment name is `nginx-deployment`

Make sure all pods are up and running after the update.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.


# Solution
`kubectl get all`


<img width="916" height="355" alt="image" src="https://github.com/user-attachments/assets/c200a560-0dea-485e-b922-5a45d631572a" />

`kubectl describe deploy nginx-deployment`


<img width="1063" height="596" alt="image" src="https://github.com/user-attachments/assets/c5c27691-0caf-4b03-9493-b14c13f08400" />

`kubectl edit deployment nginx-deployment`  change the image


<img width="918" height="392" alt="image" src="https://github.com/user-attachments/assets/e1e0b5e3-7760-471d-9937-b78243df01cb" />

`kubectl describe deploy nginx-deployment`


<img width="942" height="707" alt="image" src="https://github.com/user-attachments/assets/d7827e11-d2a3-4e81-90bd-c2f637abf4a2" />


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

Verify that the rolling update is in progress by checking the status of the deployment:

`kubectl rollout status deployment/nginx-deployment`

If the rollout has been successfully completed, you'll see output similar to the following:


<img width="703" height="43" alt="image" src="https://github.com/user-attachments/assets/5a3e5ba4-ff15-4c97-944e-e49765875a35" />

`kubectl get pods`


<img width="708" height="121" alt="image" src="https://github.com/user-attachments/assets/ff611ccd-c591-4e0f-8e96-fb339ebf3087" />
