# Instructions 

Some new requirements have come up to install and configure some packages on the Nautilus infrastructure under Stratos Datacenter. The Nautilus DevOps team installed and configured a new Jenkins server so they wanted to create a Jenkins job to automate this task. Find below more details and complete the task accordingly:

Click on the `Jenkins` button on the top bar to access the Jenkins UI. Login using username `admin` and `Adm!n321` password.

Create a Jenkins  job named `install-packages` and configure it to accomplish below given tasks.

- Add a string parameter named `PACKAGE`.
- Configure it to install a package on the `storage server` in `Stratos Datacenter` provided to the `$PACKAGE` parameter.

`Note`

1. You might need to install some plugins and restart Jenkins service. So, we recommend clicking on `Restart Jenkins when installation is complete and no jobs are running` on plugin installation/update page i.e `update centre`.
 Also some times Jenkins UI gets stuck when Jenkins service restarts in 
the back end so in such case please make sure to refresh the UI page.
