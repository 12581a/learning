# Git概述

**1.Git安装**

**2.Git入门**

新建一个文件夹，使用`git init`命令这个目录变成Git可以管理的仓库

设置系统用户级别，`git config --global user.name "Your Name"`

`git config --global user.email "email@example.com"` 

可以在.gitconfig下查看

```java
$ cat .gitconfig
[user]
        name = xxx
        email = email@2765221869@qq.com
```

**基本操作**

`git status`，查看状态

`git add [FileName]`,添加操作

`git commit -m "文件说明"`，提交

**版本穿梭**

`git log`，显示从最近到最远的提交日志，`git log --pretty=oneline` ，加上这个参数可以单行显示

`git reflog`：查看历史命令

`git reset --hard 647b18b`,基于索引值到指定的版本

`git reset --hard HEAD^`，回到上一个版本

`git reset --hard~n`,回退

`git help reset`:可以查看帮助文档

```java
reset的三个参数对比
    --soft:仅在本地库移动HEAD指针
    --mixed:在本地库移动HEAD指针,重置暂存区
    --hard:在本地库移动HEAD指针,重置暂存区和工作区
```

删除文件找回：回退到历史记录

`git diff`,比较工作区与暂存区的区别；

`git diff --cached`,比较暂存区与版本库的区别。

`git diff HEAD -- readme.txt`, 查看工作区和版本库里面最新版本的区别 

修改的操作

`git checkout -- readme.txt `

当`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

当`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

 `git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区

删除操作

执行`git rm test.txt`后，然后执行`git commit -m "remove test.txt"`

**分支操作：**

`git branch dev`，创建分支

`git checkout dev`，切换分支

`git switch dev`，切换分支(*)

`git branch -v`,查看分支

`git merge dev`,合并分支，`git branch -d dev`,删除分支。

`git stash`,保存现在的工作现场

`git stash drop` , 恢复的同时把stash内容也删了

` git stash list`

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

`git stash apply stash@{0}`

**远程仓库：**

 已经在本地创建了一个Git仓库后，又在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步 

` git remote add origin git@github.com:michaelliao/learngit.git`

`git push -u origin master`:加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

`git push origin master`,推送到远程库。

`git pull origin master`,拉取仓库到本地。

`git clone git@github.com:michaelliao/gitskills.git`,克隆仓库。

将别人的仓库fork到自己的仓库下面，然后clone到本地，做出修改后推送给别人。

`git remote -v`： 查看远程库信息。 

`git remote rm origin`：删除远程库与本地库的关联。

` git clone xxx.git`：克隆仓库

Git支持多种协议，包括https，但ssh协议速度最快。

**SSH免密登录：**

进入当前目录的家目录：`cd ~`,删除.ssh目录`rm -rvf .ssh`，运行`ssh-keygen -t rsa -C email.com`命令生成.ssh秘钥目录,将.ssh下的id_rsa.pub内容复制到gitHub中。



​		









 

































