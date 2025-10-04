# Instructions

Nautilus DevOps team is testing some applications deployment on some 
of the application servers. They need to deploy a nginx container on `Application Server 2`. Please complete the task as per details given below:

1. On `Application Server 1` create a container named `nginx_1` using image `nginx` with `alpine` tag and make sure container is in `running` state.

# Solution
ssh into the App Server 2: `ssh tony@stapp01` 
(infra: `https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details`)

Run the following command to pull the "nginx" image with the "alpine" tag from Docker Hub:

`docker pull nginx:alpine`

Execute the following command to create and run the container named "nginx_1" from the "nginx:alpine" image:

`docker run -d --name nginx_1 nginx:alpine`

Use the following command to check the status of the running container:

`docker ps`
