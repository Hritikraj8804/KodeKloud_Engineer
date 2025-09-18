# Instructions

The Nautilus application development team was working on a git repository `/opt/official.git` which is cloned under `/usr/src/kodekloudrepos`  directory present on `Storage server` in `Stratos DC`. The team want to setup a hook on this repository, please find below more details:

- Merge the `feature` branch into the `master` branch`, but before pushing your changes complete below point.
- Create a `post-update` hook in this git repository so that whenever any changes are pushed to the `master` branch, it creates a release tag with name `release-2023-06-15`, where `2023-06-15` is supposed to be the current date. For example if today is `20th June, 2023` then the release tag must be `release-2023-06-20`. Make sure you test the hook at least once and create a release tag for today's release.
- Finally remember to push your changes.

# Solution

`ssh natasha@172.16.238.15`

`cd /usr/src/kodekloudrepos/media/`

`git checkout master`

`git merge feature`

`touch /opt/media.git/hooks/post-update`

`chmod +x /opt/media.git/hooks/post-update`

`vi /opt/media.git/hooks/post-update`

```bash
#!/bin/bash
cd /opt/media.git
git_tag=release-$(date +%Y-%m-%d)
git tag $git_tag
```

`git push`

You should see a similar result:

```bash
[root@ststor01 official]# git pull
From /opt/official
 * [new tag]         release-2024-07-09 -> release-2024-07-09
```

<img width="882" height="421" alt="image" src="https://github.com/user-attachments/assets/9ffead37-4047-4afd-a91a-beff2b979d23" />
