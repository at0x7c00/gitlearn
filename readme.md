# Init and commit file
 ** git init ** :init workspace

 ** git add readme.txt ** :add file to stage

<pre>
workspace         stage
  |                |
  |git add - - - ->|
  |                |  
</pre>

 ** git commit -m 'some message'  ** : add file to local repository.
<pre>
workspace         stage                  local repository
  |                |                           |
  |git add - - - ->|git commit -m "..." - - - >|
  |                |                           |   
</pre>

 ** git status ** :look workspace status.

 ** git diff readme.txt ** : look difference between workspace and local repository.

 ** git log [--pretty# oneline][--graph] ** : look git logs.

# 版本切换 
Use  ** HEAD **  to locate commit point.
* <------HEAD(current)
* <------HEAD^
* <------HEAD^^
* <------...
* <------HEAD~100

 ** git reset --hard HEAD^ ** : reset all file to previous commit point.

 ** git reset --hard fb8a ** : reset all file to a clear submission point

<pre>
workspace         stage                  local repository
  |                |                              |
  |<- - - - - - - -|<- - - git reset --hard HEAD^ |
  |                |                              |   
</pre>

只要记得提交的版本号，就能随时在各种版本之间进行流畅的切换。

 ** git reflog ** :记录了每次操作历史

# Working tree and Stage（工作区和暂存区） 
[[Image:Git-index.png]]

Git自动创建master分支，并用HEAD指向这个分支。
这个分支就是前面提到的本地仓库（Local Repository）。

 ** 管理修改 ** ：Git管理的是修改，而不是文件内容。第一次修改后添加到stage，之后在修改，只要不添加到stage，那么commit的只有第一次修改的内容。

# 撤销修改 
<pre>
workspace                  stage                  local repository
  |                          |                              |
  |<- - - - - - - - - - - - - - - -git checkout -- <file>...|
  |                          |                              |   
</pre>

 ** git checkout -- <file>...  ** : checkout file from local repository or stage to workspace(override workspace file).工作区和暂存区同时被覆盖。

# 撤回暂存区到工作区 
<pre>
workspace                   stage                  local repository
  |                           |                              |
  |<- - -git reset HEAD <file>|                              |
  |                           |                              |   
</pre>
 ** git reset HEAD <file> ** :撤回暂存区到工作区，如果此时工作区已经存在修改的内容，那么暂存区的修改内容将和这部分合并一起成为“已修改”的内容。

撤销工作区的修改： ** git checkout -- <file>... ** 

撤销暂存区的修改： ** git reset HEAD <file> ** 

# 删除文件 
 ** git rm <file> ** : 删除文件。直接通过系统也能删除文件，但是需要使用git add配合使用。git rm把物理删除和版本系统删除两个步骤都做了。

# Remote Repository 
 ** git remote add <remote_repository_name> <remote_repository_url> ** : to add a remote_repository. 

eg. 
git remote add origin git@github.com:xooxle/gitlearn.git

 ** git push -u <remote_repository_name> <branch_name> ** : to push code on local branch to remote repository.

eg. git push -u origin master.


 ** -u的作用 ** ：可以设置默认远程分支，之后可使用 ** git push ** 代替 ** git push -u origin master ** 。


<pre>
workspace      stage        local repository                   remote repository
  |              |                    |                               |
  |              |                    |git push -u origin master- - ->|
  |              |                    |                               | 
</pre>

查看所有远程仓库: ** git remote [-v] ** 。 -v会显示更多详细信息。

不仅master分支可以push到远程分支，其他分支同样可以： ** git push origin dev ** 。

# 创建分支 
Fist,There is no branchs:

[[Image:git-no-branch.png]]


use  ** git checkout -b <branch_name> ** to create a new branch names <branch_name>.  ** -b **  means switch to it.

[[Image:git-create-new-branch.png]]

then to commit on this branch:

[[Image:git-commit-on-branch.png]]

Merge trunk to this branch:

[[Image:git-merge-trunk-to-branch.png]]

Now,delete dev branch:

[[Image:git-delete-branch.png]]


查看分支： ** git branch ** 

创建分支： ** git branch <name> ** 

切换分支： ** git checkout <name> ** 

创建+切换分支： ** git checkout -b <name> ** 

#   分支策略   
[[Image:git-branch-use-strategy.png]]

# 合并分支 

合并某分支到当前分支： ** git merge [--no-ff]<name> **  (注意方向，否则merge不成功) merge完成之后再add到暂存区以提交。

 ** '--no-ff ** ':Not Fast Forward，保留分支里的提交日志

# 删除分支 
合并之后可以顺利删除分支： ** git branch -d <name> ** 

没有合并的分支不能使用-d删除，会被告知“not fully merged”。如果一定要删除，必须使用-D，即 ** git branch -D <name> ** 

# 暂存(Stash) 
 ** git stash ** :保存现场。暂存当前所有被追踪的文件到一个stash栈中。为被track的文件不会受影响。

 ** git stash list ** :查看stash栈的内容。

 ** git stash pop ** :弹出stash栈栈顶记录，恢复现场。

 ** git stash apply ** :恢复现场，但是不删除stash栈的内容。

 ** git stash drop ** : 删除栈顶的stash记录。

这个功能非常有用。在开发过程中遇到需要紧急处理一个bug的时候，可以将手头的工作暂存起来，然后切换到其他分支处理bug。处理完成之后再回来恢复原先的工作。

另外，stash记录是对所有分支共享的，意思是可以将一个分支下的修改stash起来，然后切换到另外一个分支执行恢复操作。pop动作可以将改动移动到其他分支，apply则可将改动复制到其他分支。

# 创建关联远程分支的本地分支 
前面讲到过可以将任意本地分支push到远程仓库。如果远程仓库中有多个分支，使用 ** git clone ** 可以将这个仓库克隆到本地。克隆完成之后会发现其实只有一个master分支。如果需要切换到其他分支则需要创建一个本地分支，并且与远程分支做关联：

 ** git checkout -b dev origin/dev ** 

第一个dev就是要创建的本地分支，origin/dev表示是远程仓库中的分支。这样checkout之后，其实是将远程仓库中的dev分支加载到了本地的dev分支下。

# Pull 
pull之前需要设置远程分支与本地分支的关联：

 ** git branch --set-upstream-to# origin/dev dev ** 

然后再执行 ** git pull ** 就可以拉取文件到本地分支了。

# 多人协作流程 
1 首先，可以试图用 ** git push origin <branch-name> ** 推送自己的修改；
2 如果推送失败，则因为远程分支比你的本地更新，需要先用 ** git pull ** 试图合并；如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。
3 如果合并有冲突，则解决冲突，并在本地提交；
4 没有冲突或者解决掉冲突后，再用 ** git push origin <branch-name> ** 推送就能成功！

# Rebase 
rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

# Tag 
创建tag: ** git tag [-a] <tag_name> [-m "<tag_remark>" ] [<commit_id>] **   默认是给最新的commit创建tag，如果个特定的commit创建tag则需要制定提交的id。

查看所有tag: ** git tag ** 

查看单个tag的详细信息: ** git show <tag_name> ** 

删除tag: ** git tag -d <tag_name> ** 

推送某个本地标签到远程仓库： ** git push origin <tag_name> ** 

推送所有本地标签到远程仓库： ** git push origin --tags ** 

从远程仓库拉取tag： ** git fetch --tags ** 

删除远程仓库的标签：先删除本地的，然后执行 ** git push origin :refs/tags/<tag_name> ** 删除远程的。

# 使用GitHub 
参与开源项目的流程是
1 Fork一个开源项目，其实就是将开源项目Clone一份到了你的Github中。
2 这样你就可以随意修改了
3 修改完成之后通过pull request尝试将你的提交合并回开源项目，项目作者决定是否通过你的request

