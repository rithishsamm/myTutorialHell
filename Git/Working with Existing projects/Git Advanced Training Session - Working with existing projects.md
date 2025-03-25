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
These has been more personal working with projects all by themselves an such.   But

---

## Git Advanced - Working with existing projects

Here in this session, we will be covering a lot more on Working with others/existing projects.  
	Resources: 
	1) https://learngitbranching.js.org/
	2) https://git-school.github.io/visualizing-git/#free-remote

![[Complete Git and SCM Tutorial - KK#21) ***Merging branch to main (Learn Git Branching)***]]

![[Complete Git and SCM Tutorial - KK#22) ***Pushing new changes to master branch***]]

![[Complete Git and SCM Tutorial - KK#23) ***Working with Existing Projects***]]

![[Complete Git and SCM Tutorial - KK#24) ***Why Fork and How to Fork?***]]

![[Complete Git and SCM Tutorial - KK#25) ***Cloning the forked project to local***]]

![[Complete Git and SCM Tutorial - KK#26) ***What is Upstream and adding it to local***]]

![[Complete Git and SCM Tutorial - KK#27) ***What is a Pull Request?***]]

![[Complete Git and SCM Tutorial - KK#28) ***PUSHING A BRANCH TO GITHUB***]]

![[Complete Git and SCM Tutorial - KK#29) ***Never commit on main branch & creating our first pull request***]]

![[Complete Git and SCM Tutorial - KK#30) ***Removing a commit from the pull request by force pushing to it***]]

![[Complete Git and SCM Tutorial - KK#31) ***Merging a Pull Request***]]

![[Complete Git and SCM Tutorial - KK#32) ***Making forked project even with main project***]]

![[Complete Git and SCM Tutorial - KK#33) ***Instructions on how to try doing these on your own***]]

![[Complete Git and SCM Tutorial - KK#34) Squashing commits]]

![[Complete Git and SCM Tutorial - KK#35) ***Using the Rebase command***]]

![[Complete Git and SCM Tutorial - KK#36) ***Merge conflicts and how to resolve them?***]]


+Practicing all these with [Learn Git Branching](https://learngitbranching.js.org/), [git-school](https://git-school.github.io/visualizing-git/#free-remote) and [git-flashcards](https://www.knowledgize.com/en/d/git-cli/learn/flash-cards) We are here to cover about Extended version of these topics given above.

---
### Contents:

##### Merging branch to main (Learn Git Branching)



##### Pushing new changes to master branch



##### Working with Existing Projects



##### Why Fork and How to Fork?



##### Cloning the forked project to local



##### What is Upstream and adding it to local



##### What is a Pull Request?



##### Never commit on main branch & creating our first pull request



##### Removing a commit from the pull request by force pushing to it



##### Merging a Pull Request 



##### Making forked project even with main project



##### Instructions on how to try doing these on your own



##### Squashing commits



##### Using the Rebase command 



##### Using the hard flag to reset



##### Merge conflicts and how to resolve them?



##### What to do next?



---
### RECAP with some extra fresh content: (using  [Learn Git Branching](https://learngitbranching.js.org/))

##### Welcome to Learn Git Branching  [Learn Git Branching](https://learngitbranching.js.org/)

Interested in learning Git? Well you've come to the right place! "Learn Git Branching" is the most visual and interactive way to learn Git on the web; you'll be challenged with exciting levels, given step-by-step demonstrations of powerful features, and maybe even have a bit of fun along the way.

After this dialog you'll see the variety of levels we have to offer. If you're a beginner, just go ahead and start with the first. If you already know some Git basics, try some of our later more challenging levels.

You can see all the commands available with `show commands` at the terminal.

PS: Want to go straight to a sandbox next time? Try out [this special link](https://pcottle.github.io/learnGitBranching/?NODEMO)

PPS: GitHub has started naming the default branch `main` instead of `master` to migrate away from biased terminology [(more details available here)](https://github.com/github/renaming). In accordance with this industry-wide movement, we have also updated "Learn Git Branching" to use `main` instead of `master` in our lessons. This rename should be fairly consistent by now but if you notice any errors, feel free to submit a PR (or open an issue).

## MAIN / LOCAL / PERSONAL

### Introduction Sequence
A nicely paced introduction to the majority of git commands

**1: Introduction to Git Commits**
## Git Commits
A commit in a git repository records a snapshot of all the (tracked) files in your directory. It's like a giant copy and paste, but even better!

Git wants to keep commits as lightweight as possible though, so it doesn't just blindly copy the entire directory every time you commit. It can (when possible) compress a commit as a set of changes, or a "delta", from one version of the repository to the next.

Git also maintains a history of which commits were made when. That's why most commits have ancestor commits above them -- we designate this with arrows in our visualization. Maintaining history is great for everyone working on the project!

It's a lot to take in, but for now you can think of commits as snapshots of the project. Commits are very lightweight and switching between them is wicked fast!

Let's see what this looks like in practice. On the right we have a visualization of a (small) git repository. There are two commits right now -- the first initial commit, `C0`, and one commit after that `C1` that might have some meaningful changes.

Hit the button below to make a new commit.

```
git commit
```
it just commits.

There we go! Awesome. We just made changes to the repository and saved them as a commit. The commit we just made has a parent, `C1`, which references which commit it was based off of.

> Would you like to move on to _"Branching in Git"_, the next level?

## Git Branches

Branches in Git are incredibly lightweight as well. They are simply pointers to a specific commit -- nothing more. This is why many Git enthusiasts chant the mantra:

==**branch early, and branch often**==

Because there is no storage / memory overhead with making many branches, it's easier to logically divide up your work than have big beefy branches.

When we start mixing branches and commits, we will see how these two features combine. For now though, just remember that a branch essentially says "I want to include the work of this commit and all parent commits."

Let's see what branches look like in practice. Here we will create a new branch named `newImage`.

```
git branch newImage
```

There, that's all there is to branching! The branch `newImage` now refers to commit `C1`.

Let's try to put some work on this new branch. Hit the button below.
```
git commit
```

Oh no! The `main` branch moved but the `newImage` branch didn't! That's because we weren't "on" the new branch, which is why the asterisk (*) was on `main`.

Let's tell git we want to checkout the branch with

```
git checkout <name>
```

This will put us on the new branch before committing our changes.

```
git checkout newImage
;
git commit
```
There we go! Our changes were recorded on the new branch.

>   **Note**: In Git version 2.23, a new command called `git switch` was introduced to eventually replace `git checkout`, which is somewhat overloaded (it does a bunch of different things depending on the arguments). The lessons here will still use `checkout` instead of `switch` because the `switch` command is still considered experimental and the syntax may change in the future. However you can still try out the new `switch` command in this application, and also [learn more here](https://git-scm.com/docs/git-switch)._

Ok! You are all ready to get branching. Once this window closes, make a new branch named `bugFix` and switch to that branch.

By the way, here's a shortcut: if you want to create a new branch AND check it out at the same time, you can simply type `git checkout -b [yourbranchname]`.

> Would you like to move on to _"Merging in Git"_, the next level?


## Branches and Merging

Great! We now know how to commit and branch. Now we need to learn some kind of way of combining the work from two different branches together. This will allow us to branch off, develop a new feature, and then combine it back in.

The first method to combine work that we will examine is `git merge`. Merging in Git creates a special commit that has two unique parents. A commit with two parents essentially means "I want to include all the work from this parent over here and this one over here, _and_ the set of all their parents."

It's easier with visuals, let's check it out in the next view.

Here we have two branches; each has one commit that's unique. This means that neither branch includes the entire set of "work" in the repository that we have done. Let's fix that with merge.

We will `merge` the branch `bugFix` into `main`.
```
git merge bugFix
```

Woah! See that? First of all, `main` now points to a commit that has two parents. If you follow the arrows up the commit tree from `main`, you will hit every commit along the way to the root. This means that `main` contains all the work in the repository now.

Also, see how the colors of the commits changed? To help with learning, I have included some color coordination. Each branch has a unique color. Each commit turns a color that is the blended combination of all the branches that contain that commit.

So here we see that the `main` branch color is blended into all the commits, but the `bugFix` color is not. Let's fix that...

Let's merge `main` into `bugFix`:
```
git checkout bugFix; git merge main
```

Since `bugFix` was an ancestor of `main`, git didn't have to do any work; it simply just moved `bugFix` to the same commit `main` was attached to.

Now all the commits are the same color, which means each branch contains all the work in the repository! Woohoo!

To complete this level, do the following steps:
- Make a new branch called `bugFix`
- Checkout the `bugFix` branch with `git checkout bugFix`
- Commit once
- Go back to `main` with `git checkout`
- Commit another time
- Merge the branch `bugFix` into `main` with `git merge`

_Remember, you can always re-display this dialog with "objective"!_

.

---