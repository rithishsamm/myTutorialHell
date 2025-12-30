Previously we have conducted a training on - Introduction to Git.

Where we have covered (more of an introductory like session of Git), topic such as,
- What is Version control
- What is Git
- Git Architecture
- Local Repo
- Staging Area
- Remote Repo
- Working between local and remote repo
- What are commits
- What are branches
- Working with branches
- All the `git` commands, the essentials that are relevant to what we have covered
- and much more
These has been more personal working with projects all by themselves an such.  But

++

Here in this session, we will be covering a lot more on Working with others/existing projects.  
	Resources: 
	1) [Learn Git Branching](https://learngitbranching.js.org/)
	2) , [git-school](https://git-school.github.io/visualizing-git/#free-remote) and
	3) [git-flashcards](https://www.knowledgize.com/en/d/git-cli/learn/flash-cards) 
We are here to cover about Extended version of these topics given above.

---
## Git Advanced - Working with existing projects

In this document, I have enclosed information about how to work and collaborate with existing projects. Content including which explains in brief about forking, fetching, merging, pulling, pushing, pull requests, merge requests, branching and much more.

**For Hexr Starters: (forking and be in sync with the project)**

1. Fork the project
2. Add to connect the new remote (upstream) that you forked from to the local repo.
```
git remote add upstream parent-repo.git
```
3. fetch all the updates UpToDate: (fetches all the changes from the upstream repo)
```
git fetch upstream
```
4. Merge all those changes to your local project. ensure you are on the main branch before merging the feature branch with `main`
```
git checkout -b main #if you are new or
git switch main
git merge feature
```

```
git merge upstream/branch branch
```

=> Equivalent to (`git pull`), but it replaces your unsaved, uncommitted changes.  
To remediate that, do
```
git pull --rebase
```

which pulls all the update commits from upstream/origin to local but puts all that under your commit first and does the rest.

---
# Working and Collaborating with others/existing project

##### Terminologies
**Branches:**  
We do make commits.  
It gets saved in the form of a branch-like sequence. We can save all these commits in this multiple sequence in the name of a branch. _(based on a directed acyclic graph)_.

1. **Why Branches:** (Use of a branch)  
Whenever we are working on a new feature out of the project, KEEP A HISTORIC RECORD OF TIMELINE TO IT (code, resolving a bug, updating it to a new feature, and such). ALWAYS CREATE A SEPARATE BRANCH WHEN YOU ARE WORKING ON SOMETHING NEW.
> **Quick note**: avoid committing on main branch.  
> Always keep the code separated on multiple branch based on these updates and feature that you are working on.  
> and keep merging all these commits from branch to branch.  
> DO NOT COMMIT ON `MAIN` BRANCH, INSTEAD MERGE THE `FEATURE` BRANCHES WITH `MAIN` BRANCH UPTO DATE.

2. **HEAD**:  
Here, `HEAD` is nothing but the direction in which our project is headed/pointed.*. Telling all the commits that we are making will get added in this HEAD-pointed branch.


3. **Merge**
Merge is if you are working on a `feature` branch out of the `main` project.

Now the feature is ready, and you want to merge it into the `main` for the app to be `main`. to merge `feature` to `main`
```
git switch main
git merge feature
```

Merge all those changes to your local project; ensure you are on the `main` branch before merging the feature branch with `main` .
> Note: When you create a branch. Make sure that you are aware of the HEAD which from which branch that you are creating a new branch from.

And much more will be covered later in this page.

---

# Main Content:
##### 1) Merging branch to main (Learn Git Branching) - [[Complete Git and SCM Tutorial - KK#21) ***Merging branch to main (Learn Git Branching)***]]
If the feature is ready on `feature branch` and you want to merge it into the `main` for the application to be updated on the `main` branch, merge `feature` into `main`

Local:

```
git switch main
git merge feature 
```

Upstream:

```
git merge upstream/branch branch
git merge upstream/main main
```

After the merge, all the history of commits can be seen, and all these changes have been merged. This happens via `PR`.



##### 2) Pushing new changes to master branch - [[Complete Git and SCM Tutorial - KK#22) ***Pushing new changes to master branch***]]
and as usual.
```
git push origin main
```

All these update changes have been sent to the remote repo.


##### Working with Existing Projects -  [[Complete Git and SCM Tutorial - KK#23) ***Working with Existing Projects***]]
Now the real deal. WORKING WITH EXISTING PROJECTS: **FORK**

==Have a copy of it by forking that project under your alias/account.==


##### Why Fork and How to Fork? -  [[Complete Git and SCM Tutorial - KK#24) ***Why Fork and How to Fork?***]]

Why: Like contributing to an OSS project or collaborating with an existing project and such. To do all that, this is how you do it. by forking a project.

How to fork: By clicking the fork icon and creating a repo under your account. _This way, you can be a part of a joint project, collaborating on a bigger project. And you pay your contribution to it. This way, other code will not conflict with your code and vice versa for others._ Without these practices, if an intruder or expected error gets pushed to critical projects, it will be messed up pretty bad.

_**So, fork to your own account.**_

##### Cloning the forked project to local - [[Complete Git and SCM Tutorial - KK#25) ***Cloning the forked project to local***]]

After forking, the main repo will be called the `upstream` repo, and the forked one is our `origin` repo. Here in your repo. Do whatever you want. Which doesn't affect the `upstream` parent or main project.

This way, you cannot directly make changes in anyone’s account. To make it, fork it and apply changes, push and make a `PR` from your account from branch to branch. That `PR` will be validated by the maintainers and merged to main after approval.

##### What is Upstream and adding it to local - [[Complete Git and SCM Tutorial - KK#26) ***What is Upstream and adding it to local***]]
As we previously mentioned, the main repo will be called the `upstream` repo. To add it to our local repo in our system to `fetch`, `merge` or `pull` from. To add upstream. JUST LIKE WHAT WE DO WITH `ORIGIN`.
```
git remote add upstream upstream-repo-url.git
```

to check,
```
git remote -v
```

Now the local repo is connected to both `URLs`, as in `origin` and `upstream`.

OK. We `fetch` and `merge` branches and make our repo updated by `pushing` our changes to the `origin`. HOW DO WE PUSH THIS UPSTREAM? by making a `PR`.


##### What is a Pull Request? - [[Complete Git and SCM Tutorial - KK#27) ***What is a Pull Request?***]]
Pull request - a request that we make to `pull` all the changes from `origin` to `upstream` from a specific `branch` to `main` or a specific `feature` branch that can be merged by maintainers, namely a `pull request`. PR. How do we perform this?

*to push to origin via a branch*
```
git push origin main
```
and which gets pushed to `origin`, which is your username/reponame under your SCM.

Make a few more commits locally without pushing to origin. and check
```
git log
```

There you can see a flag to a commit like _(origin/main)_ and on the other side, which is _(HEAD → main, feature branches, … n). means,_

- _(origin/main)_ is the commit that was pushed to the `origin` at the latest.
- _(HEAD → main) is the latest and which branch it was pointed to._
- If you `git push origin main`, that label will be up here with HEAD too.

**HOW TO MAKE A PULL REQUEST?**
Say, you keep your thing on a branch committed to origin. Now, the objective is to create a `PR` to get our code from `origin` to `upstream` via a `branch` to a `branch`.
1. Go to `origin/repo`
2. Click the `pull` request button.
3. Click Create a `Pull Request`
4. Specify from which `branch` of `origin` to which `branch` of `upstream` has to be merged.
5. Now the maintainer will review your `PR`, and if all is good, the maintainer will proceed to `Merge Request`.
6. There, there will be a `commits` tab and a conversations tab to make your review easier.
7. And the `PR` will be successful in getting your `origin` branch changes merged to the `upstream` branch.
8. IF THERE ARE CONFLICTS IN THOSE, THEY CAN'T MERGE. Close PR, fix the conflicts and create a new `PR`.
Refer to the `branching` docs that I have enclosed above.

**tldr:**
- All these commits get stacked as a branch-like structure.
- can have multiple stacks of branches of commits for different phases and features of development.
- Keep separate branches for separate phases, features or cases. It means never keeping your commits directly in your `main` but keep on merging it into main from feature branches.
- Keep an eye on `HEAD`.
- To merge from feature to main, use `git switch main` + `git merge feature`.
- and push.


##### Never commit on main branch & creating our first pull request - [[Complete Git and SCM Tutorial - KK#28) ***PUSHING A BRANCH TO GITHUB***]]
PRESSING THIS AGAIN AND AGAIN.
===Maintain your versions of the application on separate, different branches===

Since we are not able to push changes to the upstream but to the `origin`. From there we can make our stuff.
==ONE BRANCH = ONE PULL REQUEST. One branch allows only one pull request.==

Make all your code distributed this way, and you are fine!

For this instance, let’s say that we did a PR from Origin’s feature to `Upstream`’s main. As in, that PR contains commits. Before the merge happens, if you commit within the merge. The PR is open still, and the commits will be added again.
> Before merging, do not commit and push which gets added to it too.

_IT WILL NOT ALLOW US TO CREATE A_ `PR` _OR THE SAME_ `PR` _AS WE DID BEFORE. But the commit gets added until it gets merged._ **Just create a branch just to add, and you will be fine.**


##### Removing a commit from the pull request by force pushing to it - [[Complete Git and SCM Tutorial - KK#29) ***Never commit on main branch & creating our first pull request***]]
Okay. Everything depends on the commits we do and some `PR` that we need to be aware of.

Let's say you made a commit by mistake. You don't want the commit to exist in the history of our project.
```
git reset commitID
git status
git log
```
Now the commit is reverted, and all the changes will be unstaged.

To keep this fresh, clear out the changes made, which will not be tracked. since, it is not tracked, we can do
```
git add .
git stash #instead of commit
```
Now it is all clear.

To pull all that back, simple `git stash pop` or to clear them `git stash clear`. To view all stashes that we do, we use `git stash ls`.

NOW THE STRUCTURE IS EXACTLY LIKE WE WISHED OR HOW IT WAS BEFORE. This way, it gets removed from commit history.


##### Merging a Pull Request - [[Complete Git and SCM Tutorial - KK#30) ***Removing a commit from the pull request by force pushing to it***]]
OKAY!? This works locally. How about upstream or at the origin? The structure is conflicting with the things that we have pushed for. Origins have something that we don't have. But to push.

To perform this (clearing cleared commits) since it is stacked and on an interlinked sequence, WE HAVE TO FORCE PUSH THIS ONE.
```
git push origin main/feature -f
```
Commit gets gone, and the same goes for files that are changed under that commit.

##### Making forked project even with main project - [[Complete Git and SCM Tutorial - KK#31) ***Merging a Pull Request***]]
`MR`. Simple. There will be a button after `PR`. After reviewing that `PR`. Merge that `PR` by clicking the `Merge Pull Request` button to confirm.

CHANGES THAT ARE MADE IN OUR PROJECT ARE NOW BEING REPRESENTED AND MERGED IN THE MAIN UPSTREAM PROJECT.


##### Instructions on how to try doing these on your own - [[Complete Git and SCM Tutorial - KK#32) ***Making forked project even with main project***]]
To be in sync with the main project. _**Making a forked project even with the main project**_. It means KEEPING UP WITH THE MAIN PROJECT.

OK. Now the local is updated. That update has been pushed to origin via a feature branch, and that got merged in the `main` branch of the `upstream`. but ==not the origin==. How to resolve this.

(Same logic that if the `origin` cannot update the `upstream` directly, how can the `upstream` update the `origin` directly?). Exactly.

There are two ways to perform this.
1. **fetch upstream button** (only on GitHub)—there will be a button to fetch from `upstream`.
2. **Manual**
_Step 1: Upstream setup_
```
git checkout main
git remote -v #check upstream
git remote add upstream upstream-repo-url.git
```

_Step 2: Fetch changes_
```
git fetch --all --prune
#safer that pull
```

_Step 3:_
1st method: (to merge branch by branch)
```
git fetch upstream #as we did that before
git merge upstream/branch branch
git push origin main
```

2nd method: (to pull from upstream)
```
git pull upstream main
git push origin main
```

3rd method: (to replicate the local as same as the upstream)
```
git reset --hard upstream/main
git push origin main
```

means, reset to what upstream’s main branch is having. Now the head will be pointed to what the Fork is having.
*safer than `git pull.*`

NOW THE FORK IS EVEN WITH THE UPSTREAM.

> **ALL ABOUT** `FORK`, `BRANCHES`, `PR`, `MR`,`FETCH`, `MERGE`,`PULL`,`PUSH`,`UPSTREAM`, `ORIGIN`.

**FROM HERE ON, LETS DIVE DEEP INTO MERGE CONFLICTS AND OTHER INTERMEDIATE TOPICS.**


##### Squashing commits - - [[Complete Git and SCM Tutorial - KK#34) Squashing commits]]
sqaush - merging multiple commits to just one commit - FOR CONVENIENCE.

```
git checkout branch #or
git switch branch
```
which may consist of mutiple commits. HOW TO MERGE IT.

**Method1: (reset and commit all to just one)**
If multiple commits that you didn’t push means, you simply `reset` these commits and commit to one single commit

**Method2: rebase** (from the commit that you want restack and merge to)
```
git rebase -i commitIDofStart
```

NOW, WE CAN EITHER **PICK** OR **SQUASH.**
replace `pick` to `s` means squash the commits you want to commit. and that will work.

`s` - merge it into previous commit above.
PERFORM YOUR EDIT. and enter
> :x

and add the message

##### Using the Rebase command - [[Complete Git and SCM Tutorial - KK#35) ***Using the Rebase command***]]



##### 14) Using the hard flag to reset to revert back from squash or deleting a commit or sum
To revert back, (commit below the squash)
```
git reset --hard  commitID 
```
and commit again.

THIS HAS BEEN A GREAT SAIL. WHAT IF WE GET CONFLICTS AND HOW TO RESOLVE THEM IF WE HAPPENED TO MEET ONE???


##### Merge conflicts and how to resolve them? - [[Complete Git and SCM Tutorial - KK#36) ***Merge conflicts and how to resolve them?***]]
**What are Merge Conflicts?**
means nothing but like,
You made a change, you made a file and been up and running on the repo.

For instance. `app.js` is written by you to code for the **Log In** functionality you wrote like 15lines for the page to be rendered. and after pushing. Someone will be having this and writing **Sign Up**, what if?

The person that writing that **Sign up** modified your code in the app.js file changing that some of that `5lines of code?

THIS IS KNOWN AS **CONFLICTS,**
> GIT WILL BE CONFUSED OF WHICH CHANGE SHOULD I TAKE FOR THIS? the one from **sign up** or **log in** for `app.js`

Git will be asking back that which change should be taken to into consideration and such since both have modified the same line number. HOW TO REMEDIATE THIS?

Say if you pushed and made a PR which wasn’t successful due to conflicts - Close the PR and revert back the conflicts and push again.

IF YOU GET A MERGE CONFLICT.
This conflict is generally indicated with the
```
<<<<< current change
...
====== end of the change
...
>>>>> incoming change
```
changes can be seen even from branch to branch.


##### What to do next? [[Complete Git and SCM Tutorial - KK#33) ***Instructions on how to try doing these on your own***]]
Just work as you are with the practices that I have just presented. And you will able to face all this and learn back by referring this document.

YOU WILL LEARN ONCE YOU FACE ALL THESE CHANGES WHILE WORKING!

WISHING YOU ALL THE BEST AND GOOD LUCK!

---
some extra fresh content: (using  [Learn Git Branching](https://learngitbranching.js.org/))

To complete this level, do the following steps:
- Make a new branch called `bugFix`
- Checkout the `bugFix` branch with `git checkout bugFix`
- Commit once
- Go back to `main` with `git checkout`
- Commit another time
- Merge the branch `bugFix` into `main` with `git merge`

_Remember, you can always re-display this dialog with "objective"!_

---

