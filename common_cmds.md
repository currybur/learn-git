# Frequently used git commands
Learn to apply git and github to manage codes.
## Installation
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```
--global will apply the name and email configuration to all repositories on your device.
## Creating a repository
```
$ mkdir learngit
$ cd learngit
$ pwd
/Users/Xiaocheng/learngit
$ git init
Initialized empty Git repository in /Users/Xiaocheng/learngit/.git/
```
## First commit
Create a new file "readme.txt" under learngit directory, then enter
```
$ git add readme.txt
$ git commit -m "wrote a readme file"
[master (root-commit) eaadf4e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
```
Can commit several files at a time.
## After Modifying
Use ```git status``` to check modifying status.
Use ```git diff readmen.txt``` to check modified contents.
## Version Backoff
Check commit log using ```git log``` or
```
$ git log --pretty=oneline
```
HEAD represents current version, HEAD^ the previous, HEAD^^ the second previous, HEAD~100. Directly use HEAD to backoff:
```
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
```
Or use version number:
```
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```
Use ```git reflog``` to see version-changing records.
## Working Directory and Repository, stage.
See [廖雪峰的git教程](https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576)
## Managing Modification
Use ```git diff HEAD -- readme.txt``` to show difference between working directory and repository.
## Cancel Modifications
Modified in working directory
```
$ git checkout -- <file>
```
Modification already added into state
```
$ git reset HEAD <file>
```
then the modification back to working directory.
If modification already committed, use version backoff to undo it.
## Delete a file
After deleting a file in working directory, we can see the differences using ```git status```. Confirm this operation,  
```
$ git rm test.txt
rm 'test.txt'

$ git commit -m "remove test.txt"
[master d46f35e] remove test.txt
 1 file changed, 1 deletion(-)
 delete mode 100644 test.txt
```
Before committing, can recover it.
```
$ git checkout -- <file>
```
## Connect to Github
Need to create SSH Key for the beginner.
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```
Don't forget you password!

And paste ```id_rsa.pub``` into "Account settings/Add SSH key"
## First push
If you already have an empty repository in github say learngit,
```
$ git remote add origin git@github.com:currybur/learngit.git
```
"origin" is the name of remote repository, can be replaced, but usually not. Then push native repository to remote.
```
$ git push -u origin master
Counting objects: 20, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (15/15), done.
Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
Total 20 (delta 5), reused 0 (delta 0)
remote: Resolving deltas: 100% (5/5), done.
To github.com:michaelliao/learngit.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```
We use -u param for the first pushing of master branch, which associates native ```master``` branch with remote ```master``` branch.
## First Clone
Have a non-empty repository,
```
$ git clone git@github.com:currybur/learngit.git
Cloning into 'gitskills'...
remote: Counting objects: 3, done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
Receiving objects: 100% (3/3), done.
```
Then a learngit directory is created.
Note that we can also use ```https://github.com/michaelliao/gitskills.git```, which is slower and needs passwords each time connecting.
## Branches Management
