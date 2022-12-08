# Git Basics

What is git for?

The purpose of git is twofold. 

1. **Collaboration**: to sync code between different machines. 
    1. If you and your friend Bob are working on the same code base on two different computers, you want to coordinate updates to the code, so it’s the same on both computers. 
    2. However, you want to essentially “chunk” changes, so that when the updates occur, they occur in such a way it doesn’t mess anything up. 
    3. This is accomplished mainly through commands `git pull` and `git push`.
2. **Version Control**: to track a history of changes
    1. Have you ever made a series of changes to your code, realize that you started going down the wrong path, and wanted to roll back everything you did in the last 2 hours? Git lets you do that. 
    2. Users chunk their changes into snapshots called “commits”
    3. This is accomplished mainly through the commands `git add` and `git commit` (commands listed below)
---    
<img width="650" alt="Screen Shot 2023-02-13 at 10 07 24 PM" src="https://user-images.githubusercontent.com/37461272/218628933-51775f6f-401c-4a7f-8efd-3b9a9d081b99.png">

Typically, your workflow will look like this. After setting up your git/github, do the following steps in order:

0. Open the terminal and navigagte using `cd` to your working repository
1. Code for a while
1. `git status` Tells you what files have been changed. Use this command as often as you want for information
1. `git diff` Tells you exactly what changes have been made. Use this command as often as you want for information
2. `git add .` This says that the changes to all files in your current directory should be considered for the next commit
3. `git commit -m '<message>'` Puts all of the changes you have made since the last commit into a snapshot, identified by the message
4. `git pull` Makes sure that any changes added to the remote (the code on github) are added to your local repo (the code on your computer)
5. `git push` Makes sure that any changes added to your local repo (the code on your computer) are added to the remote (the code on github)
5. Go back to step 1 and repeat over and over

All together, there is the local repository, the remote repository, and a local reference to the remote repository, as portrayed in this diagram. By the way, the command `git pull` is the same as doing `git fetch` (makes git "aware" of the changes on the remote repository) and then doing `git merge` (makes your local repository match those changes).

<img width="578" alt="Screen Shot 2023-02-12 at 10 46 26 PM" src="https://user-images.githubusercontent.com/37461272/218365879-dce28b83-a76d-4df8-8617-7da2d29df044.png">
