---
title: 使用hexo+github搭建个人博客
date: 2016-06-09 22:56:37
tags: [github,hexo,blog]
---

自从接触编程以来，一直想建立自己的博客来记录重要的学习笔记，这也符合“分享”的精神。但限于自己小白的身份，一直搭建不成功，直接使用github page，只是自动生成了首页，但不知如何添加自己的文章。后面了解到，这可能需要借助jekll，但试着部署，还是有种累觉不爱的感觉：折腾的精神应该提倡，但写博客应该把精力放在内容本身上，而非花在样式、不熟等等事情上（算是给自己偷懒的说辞）。后来，注意，后来接触到了**hexo**，相比jekll，它部署简单，并且实际写文章时也可以自动完成很多工作，使我可以专注写作。期间查了很多资料，在此一并做下总结。
<!-- more -->
# 1.环境准备

## 1.1 安装Git
## 1.2 安装node.js

# 2.配置Github

## 2.1 建立repository
必须先建立username.github.io，可参照github page教程

## 2.2 配置SSH-Key
貌似只有配置好SSH-Key以后才能用git命令远程控制repository？

# 3.安装Hexo
## 3.1 安装
打开Git命令控制窗口，执行下面命令
```bash
$ npm install -g hexo
```

## 3.2 快速开始

### 3.2.1 设置你的博客
在电脑中建立一个名字叫「Hexo」的文件夹（比如我建在了E:\Hexo），然后在此文件夹中右键打开Git Bash。执行下面的命令
```bash
$ hexo init
[info] Copying data
[info] You are almost done! Don't forget to run `npm install` before you start b
logging with Hexo!
```
Hexo随后会自动在目标文件夹建立网站所需要的文件。然后按照提示，运行 npm install（在 /E/Hexo下）
> windows窗口下，在那个文件夹右键点击git bash，git bash打开后默认在该文件夹目录下

```bash
npm install
```
会在E:\Hexo目录中安装 node_modules。

### 3.2.2 开始服务器（start server）
运行下面的命令（在 /E/Hexo下）
```bash
$ hexo server
[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
表明Hexo Server已经启动了，在浏览器中打开 http://localhost:4000/，这时可以看到Hexo已为你生成了一篇blog。
你可以按Ctrl+C 停止Server。
> 只有在hexo server启动时，localhost才可用

### 3.2.3 Create a new post
在/E/Hexo目录下打开git bash窗口，执行下面的命令
```bash
$ hexo new "My New Post"
[info] File created at d:\Hexo\source\_posts\My-New-Post.md
```
刷新http://localhost:4000/，可以发现在E:\Hexo\source\_posts已生成了一篇新文章 "My New Post"。

### 3.2.4 Generate static files
执行下面的命令，将markdown文件生成静态网页。
```bash
$ hexo generate
```
该命令执行完后，会在 D:\Hexo\public\ 目录下生成一系列html，css等文件。

### 3.2.5 编辑文章
hexo new "My New Post"会在D:\Hexo\source\_posts目录下生成一个markdown文件：My-New-Post.md
可以使用一个支持markdown语法的编辑器（比如 Sublime Text 2）来编辑该文件。
> 只改文件名无效，需要更改文档中的yaml文件头。

### **3.2.6 <font color=red>部署到Github</font>**

#### 3.2.6-1 部署到Github前需要配置hexo的_config.yml文件，首先找到下面的内容(一般在文档最后)
```bash
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type:
```
然后将它们修改为
```bash
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git 
  repository: git@github.com:xiaogliu/xiaogliu.github.io.git
  branch: master
```
Repository：必须是SSH形式的url（git@github.com:xiaogliu/xiaogliu.github.io.git），而不能是HTTPS形式的url（https://github.com/xiaogliu/xiaogliu.github.io.git），否则会出现错误：
> 此处有坑，有说type为github，但此处应该为git，和hexo版本有关？？？
>
> 另，有说：Be carefull, with your current configuration, you will erase all your source code on master branch. I recommend > you to deploy on `gh-pages` to be automatically reachable at https://github.com/username/repository_name
> deploy:
>    type: git
>    repo: <repository url>
>    branch: gh-pages
> http://stackoverflow.com/questions/34452547/hexoerror-deployer-not-found-github

#### 3.2.6-2 安装hexo-deployer    

```bash
npm install hexo-deployer-git --save
```

#### 3.2.6-3 deploy
```bash
$ hexo deploy
[info] Start deploying: github
[error] https://github.com/zhchnchn/zhchnchn.github.io is not a valid repositor URL!
```
使用SSH url，如果电脑没有开放SSH 端口，会致部署失败。
```bash
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
> 如果你是为一个项目制作网站，那么需要把branch设置为gh-pages。

### 3.2.7 测试
当部署完成后，在浏览器中打开http://xiaogliu.github.io/（https://xiaogliu.github.io/） ，正常显示网页，表明部署成功。

### 3.2.8 总结 
每次部署的步骤，可按以下三步来进行。
```bash
$ hexo clean
$ hexo generate
$ hexo deploy
```
> 每次写文章/修改文章都要重新全部deploy？？

### 3.2.9 总结：本地调试
#### 3.2.9-1 在执行下面的命令后，
```bash
$ hexo g #生成
$ hexo s #启动本地服务，进行文章预览调试
```
在浏览器输入http://localhost:4000，查看搭建效果。此后的每次变更_config.yml 文件或者新建文件都可以先用此命令调试，尤其是当你想调试新添加的主题时。

#### 3.2.9-2 可以用简化的一条命令
```bash
hexo s -g
```

## 3.3 命令总结

### 3.3.1 常用命令
```bash
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```

### 3.3.2 复合命令
```bash
hexo deploy -g  #生成加部署
hexo server -g  #生成加预览
```
### 3.3.4 命令的简写为
```bash
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```