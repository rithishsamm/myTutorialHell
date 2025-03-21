#### 1) Never* use git pull
PC1 / PC2

Example if PC1/PC working on a same project. 
PC1 pushed first. 
PC2 pushing later, But gets rejected due to conflicts. (To keep up with the project to push code?)

Note: 
1) Git pull - git  fetch + git merge
2) Git pull -- - git 


> You must be pulling `git pull` but BIG NO!. 
> PULLING GIT Replaces ur code that you just coded. 

Instead,
TIP:
1) do `git pull --upstream`, pulls the codes and the commits under ur changes, now commit and push the same where it gets updated. 

