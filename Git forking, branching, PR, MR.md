### Process:
![[gitFork,branch,PR,MR.excalidraw]]

#### Git Forking

###### ***Working with Existing Projects***
what is the way if there is an up and running projects on GitHub that has been developing and how do i take a part of it?

###### ***Why Fork and How to Fork?*** 
WHY, -> You should not, must be claustrophobic t manage such cases.
HOW? -> To fork it to the account -> username/project. (will see further how we can commit the same contributing to the project by pushing it all in)

###### ***Cloning the forked project to local***
```sh
git clone url
```
>- clones the project in local (personal)

###### ***What is Upstream and adding it to local***
from where you've forked this project on top of the core project. cloning a forked project:
```sh
git remote add upstream url (forked)
```


#### Git Branching
###### ***Use of branches***
WHY BRANCHES? - Each concerning changes which you've been working on, keep it in branches. KEEP THINGS ABSTRACTED!.
> [!NOTE] Note
You should never commit on a main branch unless, the branch of it gets approval after gets tested and multiple 

###### ***Making a new branch and switching to it (Learn Git Branching)***
As mention, should commit this and that, one feature and the other. keep it all in separate separate branch and work over it,.
TO CREATE A NEW BRANCH:
```sh
git branch feature (branchName)
```
> - a branch named feature gets created.
branch gets created

to work on that branch,
```sh
git checkout feature (branchName)
```
>  - HEAD that point towards and work the pointed branch
> now the main lies, gets seperated from it as feature

*HEAD - ?  a pointer, where all the changes will be made will gety committed towards Head, where the pwd git is pointing towards to / lies on.*
```sh
git checkout
```
>- is to swap between branches

###### ***Merging branch to main (Learn Git Branching)***
 LETS SAY, ALL THE CHANGES HAS BEEN FINALIZED ON THE CURRENT BRANCH (feature) 
 to merge all the changes to the main branch (where all the people are contributing and collaborating)
```sh
 git merge feature 
```
 >- now the working branch is part of the main branch
 
 people can see all these code now.
 

#### Git PR (Pull request)
) ***What is a Pull Request?***
Pull request? - lets say making a few commits.
```sh
git status
```
>  -> shows content modified (color red)

// a new branch and hopped in to work on it.
```sh
git branch rithish
```
>-  New branch has been created
```sh
git checkout rithish
```
- now the head has been pointed towards the declared branch (namely rithish)

 now all the commit will be gets into the new branch (rithish)
```sh
git add .
git commit -m "added a message
git log
```
>  -**> shows the added changes has been happened in the pointed branch. has the extra commit

###### ***PUSHING A BRANCH TO GITHUB***
```sh
git push upstream rithish
```
-> you have permission to do so and throws error. you dont have access to the main but you have access to the origin url
```sh
git push origin rithish
```
> - (since origin is your own account)

BACK TO GITHUB:

origin repo -> receives the push (says Compare and pull request) -> says whos push to where and what has been pushing. 
Review -> Create Pull request -> Done (Chnages get added and saved)
Will get the notification of request has been added +
Confirm to merge it with the project

LETS SAY MAKING ANOTHER CHANGES:
changes,
```sh
git add
git commit -m "message"
```
>(check head or if not to the corresponding branch, git checkout branch name)

```sh
git push origin rithish
```
>THIS ISNT A GOOD PRACTICE.

WHY?

###### 29) ***Never commit on main branch & creating our first pull request***

DISTRIBUTE EACH OF THE PROJECT / FEATURE HAS BEEN WORKING ON IN SEPARATE BRANCHES AND DO SEPEARTE COMMITS.

NEW BRANCH! NEW BRANCH! NEW BRANCH! For every single feature to get added. & NEW REQUEST FOR EACH AND EACH!!
> KEEP THINGS DISTRIBUTED!!
> NEVER COMMIT ON MAIN BRANCH. 
> commits will keeps on gets added to one branch for multiple projects.
> for every new feature, new bug, new issue -> CREATE NEW REQUEST 

*what to do if you wanna revert such the accidental pull requests. WHAT TO DO TO REMOVE A COMMIT*

###### 30) ***Removing a commit from the pull request by force pushing to it***
removing an extra commit. 
Just like resetting the same. here too.
```sh
git status
```
-> stats of the git
```sh
git log 
```
-> shows all the commit
```sh
git stash 
```
or make changes to revert
```sh
git reset id
```
> check log -> essentials & review all the unstaged contents (pushes commit here)
```sh
git push origin rithish -f
```
>since the content has to be removed from the repo.
> refresh
> commit gets removed.
 commit got changed, done. TO merge the changes made on the code to the repo


#### Git MR (Merge request)

###### 31) ***Merging a Pull Request***
Check PR -> Merge Pull Request -> Confirm Merge -> (changes made in the repo's corresponding branch has been reflected in the main repo.

Repo UPDATED and represented in the main project.
/done

###### 32) ***Making forked project even with main project***
SO, if any changes that are previously done has been updated by the collaborators which with the repository that we forked has not been updated. We're not keeping up updated with the project's changes.

How do we keep ourselves updated with the continuously developing project.?
Two ways:
1) Manual updates: 
Step1:
```sh
git checkout main (HEAD)
```
, - since there is no update and the branch is clean, git log and check commits. but irl, all the main branches changes has been updated (namely 3 for e.g.) commits has been made in the main project. 
- need to do is, fetch all the changes to our branch
```sh
git fetch --all --prune
```
> - gets all the changes has been made previously  will get restored in in the main branch from the main project.
changes are updated in the main branch

Step2: setting up the main branch HEAD directly to the fork repo hardened to push all the changes to the forked project.
```sh
git reset --hard upstream/main
```
>- changes has been updated from main branch of upstream main branch since the changes are someone's work.
everything has been done here are updated locally but not in remote repo.

```sh
git pull upstream main
```
>- will also pull changes happened in the upstream main to the local git.
```sh
git push origin main
```
- changes pulled from main project to local now to fork.

1) Fetch upstream, (Fetch and merge) -> changes will be updated in the forked repo.
the previous changes has been updated in the fork but not in the main which it will know that there are some changes has been made in the forked. There will be an option that gets enabled namely FETCH UPSTREAM(works like a git reset to update unfinished changes)
