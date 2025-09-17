# Instructions

`Max` want to push some new changes to one of the repositories but we don't want people to push directly to `master`branch, since that would be the final version of the code. It should always only have content that has been reviewed and approved. We cannot just allow everyone to directly push to the master branch. So, let's do it the right way as discussed below:

SSH into `storage server` using user `max`, password `Max_pass123` .  There you can find an already cloned repo under `Max` user's home.

Max has written his story about The 🦊 Fox and Grapes 🍇

Max has already pushed his story to remote git repository hosted on `Gitea` branch `story/fox-and-grapes`

Check the contents of the cloned repository. Confirm that you can see Sarah's story and history of commits by running `git log` and validate author info, commit message etc.

Max has pushed his story, but his story is still not in the `master` branch. Let's create a Pull Request(PR) to merge Max's `story/fox-and-grapes` branch into the `master` branch

Click on the `Gitea UI` button on the top bar. You should be able to access the `Gitea` page.

UI login info:

- Username: `max`
- Password: `Max_pass123`

PR title : `Added fox-and-grapes story`

PR pull from branch: `story/fox-and-grapes` (source)

PR merge into branch: `master` (destination)

Before we can add our story to the `master` branch, it has to be reviewed. So, let's ask `tom` to review our PR by assigning him as a reviewer

Add tom as reviewer through the Git Portal UI

- Go to the newly created PR
- Click on Reviewers on the right
- Add tom as a reviewer to the PR

Now let's review and approve the PR as user `Tom`

Login to the portal with the user `tom`

Logout of `Git Portal UI` if logged in as `max`

UI login info:

- Username: `tom`
- Password: `Tom_pass123`

PR title : `Added fox-and-grapes story`

Review and merge it.

Great stuff!! The story has been merged! 👏

`Note:` For these kind of scenarios requiring changes to be done in a web UI, please take screenshots so that you can share it with us for review in case your task is marked incomplete. You may also consider using a screen recording software such as loom.com to record and share your work.


# Solution

`ssh max@ststor01`

`cd story-blog`

`git branch`

```bash
max@ststor01's password: 
Welcome to xFusionCorp Storage server.
max $ ls
story-blog
max $ cd story-blog/
max (story/fox-and-grapes)$ ls
fox-and-grapes.txt  frogs-and-ox.txt    lion-and-mouse.txt
max (story/fox-and-grapes)$ git branch
  master
* story/fox-and-grapes
max (story/fox-and-grapes)$
```

Connect to Gitea as Max

<img width="943" height="276" alt="image" src="https://github.com/user-attachments/assets/c4c334a7-751c-41e7-9581-e0d235df2508" />

Open RP

<img width="948" height="303" alt="image" src="https://github.com/user-attachments/assets/a89eb244-a7c5-479a-910a-d5ce39095753" />

Change Branch and click on New Pull Request

<img width="940" height="307" alt="image" src="https://github.com/user-attachments/assets/8efedc70-c008-4b52-8487-2ba3e71653f4" />

Add PR title : Added fox-and-grapes story and Create Pull Request

<img width="942" height="440" alt="image" src="https://github.com/user-attachments/assets/b3099b58-e477-4d1f-b660-b88e0ae66913" />

Add Tom as Reviewer

<img width="942" height="431" alt="image" src="https://github.com/user-attachments/assets/4eef6b31-46c1-44de-a37a-cab6e31f3043" />

Then correct as Tom, open sarah/story-blog

<img width="941" height="230" alt="image" src="https://github.com/user-attachments/assets/bb2e19c1-9ef9-468c-8c1c-5b8bfbf1f926" />

Open Pull Requests

<img width="940" height="318" alt="image" src="https://github.com/user-attachments/assets/ce59d8da-501d-411f-a5eb-75f683aac61e" />

Merge Pull Request

<img width="941" height="420" alt="image" src="https://github.com/user-attachments/assets/cd6d16b2-b25e-44b8-9620-9cb892b48155" />

tom merged commit 1f6154cbc7 into master now

<img width="938" height="402" alt="image" src="https://github.com/user-attachments/assets/2ef4c8c5-1f9b-4e99-8c9c-11b1e9deb42c" />






