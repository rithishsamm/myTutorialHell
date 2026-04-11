GitHub tries to provide abundant storage for all Git repositories, although there are hard limits for file and repository sizes. To ensure performance and reliability for our users, we actively monitor signals of overall repository health. Repository health is a function of various interacting factors, including size, commit frequency, contents, and structure.

Git LFS: [https://git-lfs.github.com/](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbjJTbmdkMWZKM3R2LXNSblpFamlqRkRGUGtPQXxBQ3Jtc0tsRUVWd0JWS0hLSF95UWlHWnkwbGc0andvZ2xRa085RFJoeE15R2VqX1ZURjRlMlBveXpGVGpoS0VxNHktYU1qV0QzSlZYbU0ydE5LLW9MVkxnRy04SlAwZ3VSLVN3WHB5eThhMDh5QUZ6UTFhWDN1cw&q=https%3A%2F%2Fgit-lfs.github.com%2F&v=xPFLAAhuGy0)

NOTHING MUCH! 

JUST TO FOLLOW FEW STEPS,

##### Step : 1 - Git LFS install
Download and install the Git command line extension. Once downloaded and installed, set up Git LFS for your user account by running:
```
git lfs install
| 
sudo apt install git-lfs
```

##### Step : 2 - Track LFS 
In each Git repository where you want to use Git LFS, select the file types you'd like Git LFS to manage (or directly edit your .gitattributes). You can configure additional file extensions at anytime.

Define LFS File format to track em all if size >>> 100MB
```
git lfs track "*.format"
```
eg: 
.mp4
.gltf
.glsl
.h5
.iso

##### Step : 3 - track .gitattributes

After tracking the given formats, there will be a git attributes file pops up.

Now make sure .gitattributes is tracked:
```
git add .gitattributes
```


Note that defining the file types Git LFS should track will not, by itself, convert any pre-existing files to Git LFS, such as files on other branches or in your prior commit history. To do that, use the [git lfs migrate(1)](https://github.com/git-lfs/git-lfs/blob/main/docs/man/git-lfs-migrate.adoc?utm_source=gitlfs_site&utm_medium=doc_man_migrate_link&utm_campaign=gitlfs) command, which has a range of options designed to suit various potential use cases.
##### Step : 4 - all set for git-lfs, now git ritual
Just commit and push to GitHub as you normally would; for instance, if your current branch is named `main`:

```
git add file.format
git commit -m "commitMessage"
git push origin main
```

https://github.com/git-lfs/git-lfs/blob/main/docs/spec.md

