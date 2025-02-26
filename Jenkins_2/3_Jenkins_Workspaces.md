# Instructions

Some developers are working on a common repository where they are testing some features for an application. They are having three branches (excluding the master branch) in this repository where they are adding changes related to these different features. They want to test these changes on Stratos DC app servers so they need a Jenkins job using which they can deploy these different branches as per requirements. Configure a Jenkins job accordingly.

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and password `Adm!n321`.

Similarly, click on `Gitea` button to access the Gitea page. Login to `Gitea` server using username `sarah` and password `Sarah_pass123`.

1. There is a Git repository named `web_app` on Gitea where developers are pushing their changes. It has three branches `version1`, `version2` and `version3` (excluding the `master` branch). You need not to make any changes in the repository.
2. Create a Jenkins job named `app-job`.
3. Configure this job to have a choice parameter named `Branch` with choices as given below:

`version1`

`version2`

`version3`

1. Configure the job to fetch changes from above mentioned Git repository and make sure it should fetches the changes from the respective branch which you are passing as a choice in the choice parameter while building the job. For example if you choose `version1` then it must fetch and deploy the changes from branch `version1`.
2. Configure this job to use custom workspace rather than a default workspace and custom workspace directory should be created under `/var/lib/jenkins` (for example `/var/lib/jenkins/version1`) location rather than under any sub-directory etc.  The job  should use a workspace as per the value you will pass for `Branch` parameter while building the job. For example if you choose `version1` while building the job then it should create a workspace directory called `version1` and should fetch Git repository etc within that directory only.
3. Configure the job to deploy code (fetched from Git repository) on storage server (in Stratos DC) under `/var/www/html` directory. Since its a shared volume.
4. You can access the website by clicking on `App` button.

