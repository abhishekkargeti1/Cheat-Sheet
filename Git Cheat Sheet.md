# &nbsp;                		Git Cheat Sheet

###### 





git config --global user.email "email address"



git config --global user.name  "user name"



git config --list



git clone <repo link>



git status



git add 



git commit -m "message"



git remote -v   to check current fetch and push branch



git pull origin main (Use to get Remote changes on local system)



git log



1. ###### To Create A New Empty Branch

##### 

* git checkout --orphan new-empty-branch
* git rm -rf .
* echo "# Practice Branch" > README.md
* git add README.md
* git commit -m "Initial commit"
* git push -u origin new-empty-branch





###### 2\. Branch Commands 



* git branch
* git branch -M "new name of the branch " (To change the name of the existing branch)
* git checkout "branch name"  (to navigate)
* git checkout -b "branch name" (to create new branch)
* git branch -d "branch name" (to delete branch)





###### 3\. Merging Branches



* git diff "branch name"  (To compare to branches file and more )
* git merge "branch name" (To merge to branches)



or 



Create a Pull Request on GitHub



###### 4\. Undo Changes



Case 1 To change staged(add) changes

&nbsp;	git reset "File name"

&nbsp;	git restore "File Name"

&nbsp;	git reset 

&nbsp;	git restore .



Case 2 To change commited changes (for one commit)

&nbsp;	git reset HEAD~1



Case 2 To change commited changes (for many commit)

 	git reset "commit hash "

&nbsp;	git reset --hard "commit hash "









