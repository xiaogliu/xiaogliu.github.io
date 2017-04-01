---
title: Windows系统下Git配置使用
date: 2017-03-06 18:58:47
tags: [Git]
categories: 编程工具
---

## 安装
git客户端地址：[Git for Windows](https://git-scm.com/download/win)   
Git for Windows 提供了 GUI 和 Git Bash 两种操作方式，除 `merge` 差异巨大的分支外，建议都通过Git Bash进行代码维护。

## 配置
#### 配置用户名和邮箱
“这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：”
```
git config --global user.name "your name"
git config --global user.email youremail@xxx.com
```
#### 生成SSH公钥
使用git进行代码管理一般都需要远程操作，为保证安全，使用SSH公钥机制进行数据加密，完成公钥加密需要进行两步操作：
- 将自己的公钥添加至远程服务器公钥列表
若未将自己的公钥提交到远程服务器用于授权，进行 `clone` `pull` 等操作时，会提示如下错误：
```
Permission denied (publickey).
fatal: Could not read from remote repository.
Please make sure you have the correct access rights
and the repository exists.
```
将自己的公钥添加至远程服务器公钥列表前，先查看自己是否有公钥，公钥默认存放在 `~/.ssh` （波浪线代表用户的根目录，windows默认用户根目录为 `C:\Users\Administrator\` ），名称为 `id_rsa.pub`
若没有公钥，使用 `ssh-keygen` 命令生成公钥：
```
ssh-keygen -t rsa -C youremail@xxx.com
Generating public/private rsa key pair.
Enter file in which to save the key (~/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in ~/.ssh/id_rsa.
Your public key has been saved in ~/.ssh/id_rsa.pub.
The key fingerprint is:xxx
```
生成公钥过程中先要求你确认保存公钥的位置（.ssh/id_rsa），然后会让你重复一个密码两次，**如果不想在使用公钥的时候输入密码，可以留空**。

- 将远程服务器公钥添加到本地已知host列表（可能不需要用户授权，直接添加到已知host列表？）
将自己的公钥放置到远程服务器列表后，首次 `clone` 操作，会提示给远程服务器授权，比如给github授权：
```
The authenticity of host 'github.com (192.30.255.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.255.112' (RSA) to the list of known hosts.
```
输入 `yes` 后，就将 `github.com (192.30.255.112)` 添加到本地 list of known hosts。

配置完SSH公钥，就可以在本地对远程仓库进行操作了。

> .ssh目录结构及说明如下
.ssh
|-- id_rsa（秘钥，不能泄露）
|-- id_rsa.pub（公钥，提供给需要首选的远程服务器）
|-- known_hosts（已知远程服务器列表）
（关于SSH协议/RSA算法/公钥秘钥机制，再写一篇blog。）

## 解决乱码
安装完git，新建git项目后，输入 `git log` 或 `git status` 命令后中文可能出现乱码，
- 解决 `git log` 中文乱码
乱码类似
```
<E4><BF><AE><E6><94><B9><E6><96><87><E6><9C><AC><E6><96><87><E6><A1><A3>
```
或者这样
```
涓枃鍗氬娴嬭瘯
```
解决方法：打开Git Bash，输入
```
git config --global i18n.logoutputencoding gbk
```
- 解决 `git status` 中文乱码
乱码类似
```
\247\345\223\201\351\234\200\
```
解决方法：打开Git Bash，输入
```
git config --global core.quotepath false
```

> 若输入上述命令后，乱码依旧，这时可能是Git for Windows版本问题，更换其他版本即可。比如Git-2.11.0-64-bit就出现中文乱码和不能输入中文的情况，上述方法都不能解决。[Git各版本下载地址](https://github.com/git-for-windows/git/tags)

## 常用命令
基本add，commit，push，pull不做赘述，仅仅记录常用但不直观的命令

- 1、discard unstaged changes

For a specific file use:
```
git checkout <file>
```
For all unstaged files use:
```
git checkout -- .
```
Make sure to include the period at the end.

- 2、查看远程分支url
```
git remote -v
```
- 3、clone远程分支到本地某新建分支
```
git clone -b <lacal-branch-name> <target-url>
```
- 4、查看本地分支
```
git branch
```
- 5、查看远程分支
```
git branch -r
```
- 6、新建分支，并停留在当前分支
```
git branch <branch-name>
```
- 7、切换分支，并更新工作区
```
git checkout <branch-name>
```
- 8、新建分支，并与远程分支建立追踪关系
```
git branch --track <branch-name> <remote-branch-url>
```
- 9、合并远程分支
```
git merge <remote-branch-name>(一般是origin/branch-name)
```

#### 参考资料
【1】[How do I discard unstaged changes in Git?](http://stackoverflow.com/questions/673407/how-do-i-clear-my-local-working-directory-in-git)   
【2】[常用 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html?bsh_bid=938838579)
【3】[起步 - 初次运行 Git 前的配置](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE)