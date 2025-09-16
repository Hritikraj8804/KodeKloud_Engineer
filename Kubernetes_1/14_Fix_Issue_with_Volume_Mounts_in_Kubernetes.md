# Instructions

We deployed a Nginx and PHP-FPM based setup on Kubernetes cluster last week and it had been working fine so far. This morning one of the team members made a change somewhere which caused some issues, and it stopped working. Please look into the issue and fix it:

The pod name is `nginx-phpfpm` and configmap name is `nginx-config`. Figure out the issue and fix the same.

Once issue is fixed, copy `/home/thor/index.php` file from the `jump host` to the `nginx-container` under nginx document root and you should be able to access the website using `Website` button on top bar.

`Note:` The `kubectl` utility on `jump_host` has been configured to work with the kubernetes cluster.

# Solution

`kubectl get all -o wide`

<img width="1247" height="206" alt="image" src="https://github.com/user-attachments/assets/0386d564-8edd-478f-9c82-14b3a6c1619e" />

`kubectl describe configmap nginx-config`

<img width="567" height="640" alt="image" src="https://github.com/user-attachments/assets/0f523309-51a4-4e1a-9978-08c8f4254abd" />

This command retrieves the YAML definition of the nginx-phpfpm pod and saves it to the /tmp/nginx.yaml file. By examining the pod's configuration, we can identify the specific location where the file needs to be copied:

`kubectl get pod nginx-phpfpm -o yaml  > /tmp/nginx.yaml`

This command displays the contents of the /tmp/nginx.yaml file. It helps in inspecting the pod's YAML definition to locate the mountPath for the nginx container's document root:
`cat /tmp/nginx.yaml`

<img width="945" height="131" alt="image" src="https://github.com/user-attachments/assets/3b324beb-52ad-4053-8bf6-a3fa118337ca" />

