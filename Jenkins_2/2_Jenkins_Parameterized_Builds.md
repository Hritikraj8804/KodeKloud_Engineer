# Instructions

A new DevOps Engineer has joined the team and he will be assigned some Jenkins related tasks. Before that, the team wanted to test a simple parameterized job to understand basic functionality of parameterized builds. He is given a simple parameterized job to build in Jenkins. Please find more details below:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

1. Create a `parameterized` job which should be named as `parameterized-job`

2. Add  a `string` parameter named `Stage`; its default value should be `Build`.

3. Add a `choice` parameter named `env`; its choices should be `Development`, `Staging` and `Production`.

4. Configure job to execute a shell command, which should echo both parameter values (you are passing in the job).

5. Build the Jenkins job at least once with choice parameter value `Production` to make sure it passes.

`Note:`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`. Also, Jenkins UI sometimes gets stuck when Jenkins service restarts in 
the back end. In this case, please make sure to refresh the UI page.

2. For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as [loom.com](http://loom.com/) to record and share your work.

# Solution

Create a new job
![1](https://github.com/user-attachments/assets/9c7204d4-48b6-4078-8cc3-34d642370316)

add String Parameter
![2](https://github.com/user-attachments/assets/366bc422-7c39-4964-b31c-2dc8aadf4be7)

And Choice Parameter
![3](https://github.com/user-attachments/assets/61affb4d-b29f-4b69-b02e-e873aec9af1a)

And Build steps
![4](https://github.com/user-attachments/assets/7c4b988d-82f5-4e68-8f3e-3e4cc40f5bc3)

Apply → Save

Open Build with Parameters and select Production, click Build
![5](https://github.com/user-attachments/assets/ad20375d-ef01-4499-8bcc-48c7e2bee179)

Check the result
![6](https://github.com/user-attachments/assets/c9692cdd-4ecb-42bb-8653-eea18078e46e)
![7](https://github.com/user-attachments/assets/bac239fa-cf13-41f4-b765-cc0c3474a69e)
