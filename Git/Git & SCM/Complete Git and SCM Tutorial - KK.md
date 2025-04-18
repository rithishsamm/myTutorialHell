description:
This tutorial will help you with using Git & SCM (Source control manager) for your personal projects and contributing to Open Source, with no prerequisites, in an easy to understand language.

It starts from the very basics of Git & SCM Platforms, covering all the essential commands, including concepts such as branching, pull requests, forking, etc. We also cover other concepts such as squashing, resolving merge conflicts, keeping your code in sync, and more. 

NOTE: 
>**Here, will use GitHub as the SCM Platform here for reference. Doesn't matter and mostly the rest of the SCM will be the same as we do all things here.**
> 
Windows users can use git bash, all the commands will work. - When using git for the first time, set the global configs using the commands: 
```
$ git config --global user.name "your_name"
$ git config --global user.email "your_email"
```

Git/GitHub cheat sheet: [https://education.github.com/git-chea...](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbmxLVXhzOFJmWHo5TkhtY2NGaWgtaDllcjJCUXxBQ3Jtc0tseHJHcXBKMlVOeGJuVFhRX1dINGROaDQxYUJsQkg4YTFhV1NwNnpfSGdYenA4R0ZwS254SGVuTTd6Zm5xV2M1RjhyaDZVWUx1T1pJNXN6OXJWcEJ6cEpGZ2R3MEFCUXF6VnlNNlhyQzVta2JWWWhoUQ&q=https%3A%2F%2Feducation.github.com%2Fgit-cheat-sheet-education.pdf&v=apGV9Kg7ics) 

Download Git: [https://git-scm.com/downloads](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa2hhX0FJTWVjT2JBTUVfcTFYUW9SLTMxeFB0d3xBQ3Jtc0tsYkttNGczQmlGOF9zN1JxdVFEcEpicDF2TGpSakt0emtMcEN0MzFUSlVYQldxQWtrTE1IYWY5emZaVXRtdThpUnd0Zk84WFc4T2tkWXUwRWxkMlhGb09uRk5scWFoNHBIOEFkU1R2TXM5dEhkV1lvdw&q=https%3A%2F%2Fgit-scm.com%2Fdownloads&v=apGV9Kg7ics)

Cheat sheet: https://www.instagram.com/p/DFLJR3PIuEZ/?utm_source=ig_web_copy_link&igsh=MzRlODBiNWFlZA==

**Table of contents:**
**Timestamps:**
- Introduction
- What is Git and GitHub?
- Why are we using Git and GitHub?
- Downloading Git
- Structure of the Tutorial
- Some basic Linux commands
- Initializing a Git Repository
- Making the first change
- Staging the first change
- Committing the first change
- Adding data to files
- Removing changes from stage
- Viewing the overall history of the project
- Making few more commits
- Removing a commit from the history of a project
- Stashing changes
- Popping Stash
- Clearing Stash
- Starting GitHub
- Creating a new repository on GitHub
- Connecting Remote Repository to Local Repository
- Pushing local changes to remote repository
- What are branches?
- Use of branches
- Making a new branch and switching to it (Learn Git Branching)
- Merging branch to main (Learn Git Branching)
- Pushing new changes to master branch
- Working with Existing Projects
- Why Fork and How to Fork? 
- Cloning the forked project to local
- What is Upstream and adding it to local
- What is a Pull Request?
- Never commit on main branch & creating our first pull request
- Removing a commit from the pull request by force pushing to it
- Merging a Pull Request
- Making forked project even with main project
- Instructions on how to try doing these on your own
- Squashing commits 
- Using the hard flag to reset
- Merge conflicts and how to resolve them?
========================================= 
#### Introduction
Complete Git and GitHub 101.
#### What is Git and GitHub?
GIT?
-- Imagine having a project or project folder or Source Code in hand. You want to collaborate with other people contributing to that project.
-- Launched an app and you wanna add updates to it. or revert what has been existed to roll back and run the same
BASICALLY SAVING THE HISTORY OF THE PROJECT where at this time, the status of the project is this, this time that and more. Going back and forth time machine of the project.
-- Contributing to an open project where bunch of people adding bunch of code over one project and do know all the stats of who did it, when and where in the project.

**GITHUB?** - Platform for host and managing repositories. Source control manager for all the projects around the world for the public. Git based services providing Git functionalities with a user friendly convenient interface for using such service. 

**Repositories?** - is a folder where all the changes will get saved.

**Available similar options/alternatives?** - Gitlab, Bitbucket, Gitea and more

Will use GitHub for our convenience.
#### Why are we using Git and GitHub?
Why GitHub:
- maintaining the history of the project
- change management
- and sharing the project to the public and
- be a part of other's projects too.
-- any project ->  check commits -> Refer the history of the project.
that's basically the idea of what we're gonna cover later on.
#### Downloading Git
Search and download: git-scm

#### Structure of the Tutorial
Manipulating structures of files and folders on the file system using GIT to manage project and source files.
Terminal on left and file system on right.

#### Some basic Linux commands
in terms of file management such as ls, mkdir, cd, touch, cat
GIT:
where is the entire history of the changes, adding, deleting,   Code has been stored!?,  in an another that git provides us as a repository and does all the GIT's actions here locally.

###### **1)  *Initializing a Git Repository***
```sh
git init
```
 - initiates git on that dir (remains hidden in .dot)
###### **2)  *list all***
```sh
ls -a
```
(or) 
```sh
ls .git
```
-> shows contents
```sh
touch name.txt
```

now we will be able to maintain the history of the project. 
###### 3) ***Making the first change***
ANY CHANGES WILL BE MADE IN THIS PROJECT NOW WILL BE TRACED AND TRACKED BY GIT.
> if there has some changes been made and if you want to find out which and which aren't will be shown in  
```sh
git status
```
-> *untraced file* - untracked content will be shown (colors in red)
> (shows all the updated changes been made of the git that just initiated)

###### 4) ***Staging the first change***
TO SAVE/TRACK SUCH CHANGES?
what to do if we want to track and maintain changes with the help of git? e.g.: imagine a wedding function, 
```sh
git add . 
```
>  (or)
```sh
git add filename
```
>  name,txt-> (. current pwd's content or individual declaration to maintain one at a time)
```sh
git status
```
- *new file added* - to check the tracked in contents (colors in green)
###### 5) ***Committing the first change***
```sh
git commit -m "message"
```
-- commits changes to GIT
 **git status** - all good, no changes exist to commit, the tree is clean
 
**GIT RITUAL:** 
~~~sh
- git add .
- git commit 
- git status
~~~

###### 6) ***Adding data to files***
make extra changes by adding some context inside a file. name.txt -> names -> save

```sh
git status
```
: modified -text
```sh
git add  .
```
> (adds changes to git)
```sh
git commit .
```
>  (commits changes to staged area)
```sh
git status
```
-- added (color in green)

###### 7) ***Removing changes from stage***
incase if you want to revert back a change from git,
```sh
git restore --staged . (or) filename
```
```sh
git status
```
: modified: back to unchanged (color in red) 

###### 8) ***Viewing the overall history of the project***
the entire history of you!
```sh
git log
```
> -> shows all the commits of what has been add or removed

###### 9) ***Making few more commits***
MORE COMMITS:
imagine deleting a file. that we've been working on. 
```sh
rm -rf filename** (name.txt)
```
>  - removes corresponding file
```sh
git status
```
> -> file deleted (color in red)
```sh
git add .** or filename,txt
```
```sh
git commit -m "the file has been deleted"
```
```sh
git log 
```
> -> filename.txt gets deleted  among the history of changes

 but, Like, Unfortunately, imagine you **deleted the project** by mistake. WHAT YOU CAN DO? like simply i want to remove that one action on my history of the project. HOW DO I OD THAT!?
 > you cannot remove as one such log in that middle commit of that particular project.

> In **git log** - each of that commit has its own commit ID where it adds **on top of each other so that it can track one step at a time.** 
>  in this case of breaking a commit in-between is a joke. but you can revert who and how it was been there. 

###### 10) ***Removing a commit from the history of a projectTO DO THAT!?***
>  You can unstage the commits and revert it back to its previous state. to acheive that, 

```sh
git reset commitID
```
> -> reverts back to that stage and keep the committed files remains unadded BACK TO UNSTAGED AREA

TO RESTORE THE FILES,
```sh
git restore filename
```
> - to discard the changes happened over git
THAT WE ID GETS REMOVED AND REVERTED!!

###### 11) ***Stashing changes***
to archive files later to be staged to added in git/ keeping it all backstage to commit later.
***I DONT WANT TO COMMIT THIS FOR NOW + ALSO I DONT WANT TO LOOSE THIS CHANGES*** - like a new alias 
to put this into perspective where, if you'd like to try out something new with a clean codebase of not affecting the current progress, Stash in a place where keep it all archived and work on a new clean codebase for experimentation and get back after a failed and successful implementation.
```sh
git add .
```
```sh
git status
```
>  - files deleted
```sh
git stash
```
> - stashes the changes that has been added and shown in the status
```sh
git status 
```
- clean, work it all in new
```sh
git log 
```
- going back to project

###### 12) ***Popping Stash***
to pop the stash which has been archived (getting the added stashed changes back)
```sh
git stash pop
```
>  - changes which has been made previously has been restored 

###### 13) ***Clearing Stash***
incase, the experiment has been successful that u don't even want the stash, (clearing/deleting a stash which is not required anymore)
```sh
git stash clear
```
>  - clear stash

###### 14) ***Starting GitHub***
basics has been covered, NOW GETTING STARTED WITH GITHUB,
Objectives:
- Hosting your own project on GitHub, sharing it with other people (Local to remote repo)
and ..

###### 15) ***Creating a new repository on GitHub***
- New Repo -> repoName -> Public -> Create Repo
- URL -> the URL of the project/repository

###### 16) ***Connecting Remote Repository to Local Repository***
to ATTACH THE REMOTE REPO TO THE WORKING PROJECT,
```sh
git remote add origin
```
>- to connect a tunnel between the local git repo and the remote GitHub repo
>dissecting the cmd - git - calls git, remote - working with url, add - adding what URL, origin - what is the url name which you're going to add on 
```sh
git remote -v
```
>-> shows all the URL origin exists in the git

now the local project has been connect to the url.

###### 17) ***Pushing local changes to remote repository***
PUSHING CHANGES TO THE REPO,
```sh
git push origin master(or any branch)
```
> - current changes got pushed and all the stats can be seen over GITHUB

###### 18) ***What are branches?***
```sh
touch name.txt
```
BRANCH - Imagine you are making commits. 
eg: make few commits for example, 
change, add, commit with message x4
```sh
git log 
```
-> each has its own id + shows the branch(origin/master). 
EACH git commit as mentioned, it lies on top of each other + linked to each other on top of one one step at a time - (programmatic method of stacking this one on another named **directed acyclic graph**)  . LIKE A BRANCH STRUCTURE.
namely here, master. OKAY? WHAT IS THE USE OF IT?

###### 19) ***Use of branches***
WHY BRANCHES? - Each concerning changes which you've been working on, keep it in branches. KEEP THINGS ABSTRACTED!.
> [!NOTE] Note
You should never commit on a main branch unless, the branch of it gets approval after gets tested and multiple 

###### 20) ***Making a new branch and switching to it (Learn Git Branching)***
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

###### 21) ***Merging branch to main (Learn Git Branching)***
 LETS SAY, ALL THE CHANGES HAS BEEN FINALIZED ON THE CURRENT BRANCH (feature) 
 to merge all the changes to the main branch (where all the people are contributing and collaborating)
```sh
 git merge feature 
```
 >- now the working branch is part of the main branch
 
 people can see all these code now.
 
###### 22) ***Pushing new changes to master branch***
Push new changes (All the commits that hasn't been added) to the Remote repo.
```sh
git push origin master
```
-> to push all the locally committed changes to the repo 

check SCM. done!
all the project has been shipped to GitHub with all the commit histories
```sh
git log
```
>+ commits of the project = equalizes

###### 23) ***Working with Existing Projects***
what is the way if there is an up and running projects on GitHub that has been developing and how do i take a part of it?

###### 24) ***Why Fork and How to Fork?*** 
WHY, -> You should not, must be claustrophobic t manage such cases.
HOW? -> To fork it to the account -> username/project. (will see further how we can commit the same contributing to the project by pushing it all in)

###### 25) ***Cloning the forked project to local***
```sh
git clone url
```
>- clones the project in local (personal)

###### 26) ***What is Upstream and adding it to local***
from where you've forked this project on top of the core project. cloning a forked project:
```sh
git remote add upstream url (forked)
```

###### 27) ***What is a Pull Request?***
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

###### 28) ***PUSHING A BRANCH TO GITHUB***
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
> //sine the content has to be removed from the repo.
> refresh
> commit gets removed.
/- commit got changed, done. TO merge the changes made on the code to the repo

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

###### 33)  ***Instructions on how to try doing these on your own***
fork and exercise all the same has been done over here.

###### 34) Squashing commits 
Will cover more in * 
1) Squashing Commits (Merging Commits into one single commit)
2) Merge Conflict
-- 
>whenever we create a branch, it will automatically falls under HEAD. 

so
>before that make sure that all the commits and changes updated on all those branchese especially Main and master.

###### 35) ***Using the Rebase command***
-- squashing into multiple commits into one.
```sh
 git branch newBranch 
 git checkout branchName
```
--> do all the multiple commits all the 
```sh
git add .
git commit -m "message" 
```
xNo.of times
```sh
git log
```

one, we can git reset and stash and restore the same.
other one is to squash, we can use rebase command for that.
```sh
git rebase -i commitIDofTheCommitToSquash**
```
> :x
> xxx //commit message
> :x
```sh
git log
```
- check all the squashed commits

now all the commits above it will be shown in the interactive terminal that  YOU CAN EITHER PICK OR SQUASH EACH COMMITS. (pick / s). 
pick the commits that are below commits and squash the rest to the following commits.
```
pick dfsf98fa msg1
s fdsdjs098 msg2
s fkj09fwjff msg3
pick fkibvs83c msg4
```
// squashes will fall under the following commit.

33) ***Using the hard flag to reset***
```sh
git push origin temp
```
-- can be pushed to the project.
```sh
git reset --hard commitID
```
-- RESETS
```sh
git log
```
- multiple branches has the same squashed commit

###### 36) ***Merge conflicts and how to resolve them?***
Merge conflicts - if I am making a change on line number 4 + some other person is also makin g a change on the same line number 4 means = git will get confused on which one to pick and proceed further in.

lets perform a merge conflict,
1) make a change in the main project, create a new branch and commit
2) make the same change over here in local, 
3) 
```
git add/ git commit
``` 
and
4) 
```
git push origin branchName
```
5) Compare and pull request -> PR -> Review-> Create pull request
6) MERGE CONFLICT (provides info with the file) ->  Resolve Conflicts -> 
make changes and Mark has resolved -> merge Pull Request + Confirm Merge -> Merged Successfully and changes got reflected.
###### 37) What to do next?
all good! practice the same.

-fin-