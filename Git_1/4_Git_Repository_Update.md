# Instructions

The Nautilus development team started with new project development. They have created different Git repositories to manage respective project's 
source code. One of the repositories `/opt/demo.git` was created recently. The team has given us a sample `index.html` file that is currently present on `jump host` under `/tmp` directory. The repository has been cloned at `/usr/src/kodekloudrepos` on `storage server` in `Stratos DC`.

Copy sample `index.html` file from `jump host` to `storage server` under cloned repository at `/usr/src/kodekloudrepos/demo`, further `add/commit` the file and push to the `master` branch.

# Solution

Infra: `https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#infrastructure-details`


Check the file location


Copy file: `sudo scp /tmp/index.html natasha@ststor01:/tmp`

ssh into the Storage server: `ssh natasha@ststor01`

Copy file to the destination: `cp index.html /usr/src/kodekloudrepos/demo`

Go to `cd /usr/src/kodekloudrepos/demo`

`git status`

`git add index.html`

`git commit -m "add index.html"`

`git push origin master`
