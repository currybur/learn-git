# Git Learning Notes
**菜鸡学git笔记以及经常遇到的一些tips**
***
Learn to apply git and github to manage codes.
***
## 本地操作

* 初始配置用户信息
  ```
  $ git config --global user.name "Your Name"
  $ git config --global user.email "email@example.com"
  ```
  --global will apply the name and email configuration to all repositories on your device.
* 创建一个仓库
  ```
  $ mkdir learngit
  $ cd learngit
  $ pwd
  /Users/Xiaocheng/learngit
  $ git init
  Initialized empty Git repository in /Users/Xiaocheng/learngit/.git/
  ```
* First commit  
  
  Create a new file "readme.txt" under learngit directory, then enter
  ```
  $ git add readme.txt
  $ git commit -m "wrote a readme file"
  [master (root-commit) eaadf4e] wrote a readme file
  1 file changed, 2 insertions(+)
  create mode 100644 readme.txt
  ```
  Can commit several files at a time.  

* add之后可以查看修改的内容

  Use ```git status``` to check modifying status.
  Use ```git diff readmen.txt``` to check modified contents.

* Version Backoff

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

* Working Directory and Repository, stage.
  
  See [廖雪峰的git教程](https://www.liaoxuefeng.com/wiki/896043488029600/897271968352576)

* Managing Modification
  
  Use ```git diff HEAD -- readme.txt``` to show difference between working directory and repository.

* 撤销修改

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
  
* Delete a file
  
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

And paste ```id_rsa.pub``` into "Account settings/Add SSH key" on github.

* First push

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

* First Clone

  Have a non-empty repository,
  ```
  $ git clone git@github.com:currybur/learngit.git
  Cloning into 'gitskills'...
  remote: Counting objects: 3, done.
  remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
  Receiving objects: 100% (3/3), done.
  ```
  Then a learngit directory is created.
  Note that we can also use https, which is slower and needs passwords each time connecting.

* Branches Management
  
  Create a new branch.
  ```
  $ git checkout -b dev
  Switched to a new branch 'dev'
  ```
  which is equal to
  ```
  $ git branch dev
  $ git checkout dev
  Switched to branch 'dev'
  ```
  Check the current branch.
  ```
  $ git branch
  * dev
    master
  ```
  After finishing the work on dev branch, we can go back to master branch and merge the result.
  ```
  $ git merge dev
  Updating d46f35e..b17d20e
  Fast-forward
  readme.txt | 1 +
  1 file changed, 1 insertion(+)
  ```
  Then delete dev branch.
  $ git branch -d dev
  Deleted branch dev (was b17d20e).
  We can also use switch command instead of checkout. For example, create and switch to a new branch
  ```
  $ git switch -c dev
  ```
  Just want to switch to a currently existing branch
  ```
  $ git switch master
  ```

* Dealing with Conflict

  After modifying on different branches respectively, we can not directly merge them. Can go to the file where the conflict occurs, and fix it. Use ```git log``` to check branches' merging information.
  ```
  $ git log --graph --pretty=oneline --abbrev-commit
  *   cf810e4 (HEAD -> master) conflict fixed
  |\  
  | * 14096d0 (feature1) AND simple
  * | 5dc6824 & simple
  |/  
  * b17d20e branch test
  * d46f35e (origin/master) remove test.txt
  * b84166e add test.txt
  * 519219b git tracks changes
  * e43a48b understand how stage works
  * 1094adb append GPL
  * e475afc add distributed
  * eaadf4e wrote a readme file
  ```

* Branch Managing Policy
  
  ```git merge``` will be executed in ```Fast forward``` mode, which throws the information of branching away. We can use the ```git merge --no-ff``` parameter.

* Storing Working Scene on a Branch
  
  If we have work unfinished on dev branch, and have to do another task
  ```
  $ git status
  On branch dev
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

    new file:   hello.py

  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

    modified:   readme.txt
  ```
  Then use ```git stash``` to store them. After another task, to ressume the stored work, first check
  ```
  $ git stash list
  stash@{0}: WIP on dev: f52c633 add merge
  ```
  Here we have ```git stash apply``` to recover and not delete it from stash, ```git stash drop``` is needed. Or use ```git stash pop```. Can stash particular task.
  ```
  $ git stash apply stash@{0}
  ```
* Copy Modification Commit to Other Branches

  After committing on master branch, we want to repeat on dev branch.
  ```
  $ git branch
  * dev
    master
  $ git cherry-pick 4c805e2
  [master 1d4b803] fix bug 101
  1 file changed, 1 insertion(+), 1 deletion(-)
  ```

* Developing a New Feature
  
  Should better create a new feature branch. If we want to abandon it before merging, 
  ```
  $ git branch -D feature-vulcan
  Deleted branch feature-vulcan (was 287773e).
  ```

* Cooperation
  
  Try to push my modification
  ```
  $ git push origin <branch-name>
  ```
  Failed for the remote being newer than my local, so try to merge
  ```
  $ git pull
  ```
  If there are conflicts, solve it and commit locally, then
  ```
  $ git push origin <branch-name>
  ```

## 一些tips

* 报错```fatal: refusing to merge unrelated histories```，如果合并了提交历史不同的仓库，为了防止程序员手残，git就会给这么个提示。确认后使用
```--allow-unrelated-histories```来完成merge。
* 有时候发现commit没记录了，可能是提交的用户邮箱和github上留的不一样，```git config user.name/user.email```查看，不对就
  ```
  $ git config --global user.??? "your name/email"
  ```
  也可以把之前错误的提交记录全改了，首先新clone一份仓库
  ```
  $ git clone --bare your-repo
  $ cd your-repo.git
  ```
  然后粘贴这个脚本
  ```c
    #!/bin/sh
    git filter-branch --env-filter '
    OLD_EMAIL="旧的Email地址"
    CORRECT_NAME="正确的用户名"
    CORRECT_EMAIL="正确的邮件地址"
    if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
    then
        export GIT_COMMITTER_NAME="$CORRECT_NAME"
        export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
    fi
    if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
    then
        export GIT_AUTHOR_NAME="$CORRECT_NAME"
        export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
    fi
    ' --tag-name-filter cat -- --branches --tags
  ```
  回车，就把这个clone里的提交历史改了，然后push
  ```
  $ git push --force --tags origin 'refs/heads/*'
  ```
  卸磨杀驴删掉这个clone
  ```
  $ cd ..
  $ rm -rf repo.git
  ```
