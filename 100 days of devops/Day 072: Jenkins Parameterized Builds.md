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
<img width="1197" height="292" alt="image" src="https://github.com/user-attachments/assets/2071b936-2375-408e-8b08-39342d12e0ed" />

add String Parameter
<img width="1168" height="366" alt="image" src="https://github.com/user-attachments/assets/dd3c47f8-8c8f-4e67-ad11-30c22c3c1244" />

And Choice Parameter
<img width="1208" height="337" alt="image" src="https://github.com/user-attachments/assets/14b5f266-e926-40a4-999e-e8aa477251b2" />

And Build steps
<img width="1037" height="348" alt="image" src="https://github.com/user-attachments/assets/f33bc732-c116-488c-a8d4-941bcff247b4" />

Apply → Save

Open Build with Parameters and select Production, click Build
<img width="913" height="345" alt="image" src="https://github.com/user-attachments/assets/d96767a6-1a0a-413e-aa3d-fde16141bffe" />

Check the result
<img width="1005" height="386" alt="image" src="https://github.com/user-attachments/assets/f02284e2-a430-4f27-96d5-8fd7db14e378" />
<img width="502" height="228" alt="image" src="https://github.com/user-attachments/assets/b9873a61-9045-4655-a2e8-a22701737de7" />
