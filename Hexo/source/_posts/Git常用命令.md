---
title: Git常用命令
date: 2017-03-06 18:58:47
tags: [Git]
categories: 工具
---
基本add，commit，push，pull不做赘述，仅仅记录常用但不直观的命令

#### 1、discard unstaged changes

For a specific file use:
```
git checkout <file>
```
For all unstaged files use:
```
git checkout -- .
```
Make sure to include the period at the end.

#### 2、查看远程分支url
```
git remote -v
```
#### 3、clone远程分支到本地某新建分支
```
git clone -b <lacal-branch-name> <target-url>
```
#### 4、查看本地分支
```
git branch
```
#### 5、查看远程分支
```
git branch -r
```
#### 6、新建分支，并停留在当前分支
```
git branch <branch-name>
```
#### 7、切换分支，并更新工作区
```
git checkout <branch-name>
```
#### 8、新建分支，并与远程分支建立追踪关系
```
git branch --track <branch-name> <remote-branch-url>
```

#### 参考资料
【1】[How do I discard unstaged changes in Git?

](http://stackoverflow.com/questions/673407/how-do-i-clear-my-local-working-directory-in-git)
【2】[常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html?bsh_bid=938838579)