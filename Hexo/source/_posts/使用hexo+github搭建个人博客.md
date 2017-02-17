---
title: 使用hexo+github搭建个人博客
date: 2016-06-09 22:56:37
tags: [git,hexo,blog]
categories: 工具
comments: false
---

记录利用hexo搭建blog的过程。

自从接触编程以来，一直想建立自己的博客来记录重要的学习笔记，这也符合“分享”的精神。但限于自己小白的身份，一直搭建不成功，直接使用 [github page](https://pages.github.com/)，只是自动生成了首页，但不知如何添加自己的文章。后面了解到，这可能需要借助 [jekll](http://jekyllrb.com/) ，但试着部署，还是有种累觉不爱的感觉：折腾的精神应该提倡，但写博客应该把精力放在内容本身上，而非花在样式、不熟等等事情上（算是给自己偷懒的说辞）。后来，注意，后来接触到了 [**hexo**](https://hexo.io/) ，相比jekll，它部署简单，并且实际写文章时也可以自动完成很多工作，使我可以专注写作。期间查了很多资料，在此一并做下总结。

<!-- more -->

## 一、环境准备

### 1.1 安装Git
### 1.2 安装node.js

## 二、配置Github

### 2.1 建立repository
必须先建立username.github.io，可参照github page教程

### 2.2 配置SSH-Key
貌似只有配置好SSH-Key以后才能用git命令远程控制repository？

## 三、安装Hexo
### 3.1 安装
打开Git命令控制窗口，执行下面命令
```bash
$ npm install -g hexo
```

### 3.2 快速开始

#### 3.2.1 设置你的博客
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

#### 3.2.2 开始服务器（start server）
运行下面的命令（在 /E/Hexo下）
```bash
$ hexo server
[info] Hexo is running at http://localhost:4000/. Press Ctrl+C to stop.
```
表明Hexo Server已经启动了，在浏览器中打开 http://localhost:4000/ ，这时可以看到Hexo已为你生成了一篇blog。
你可以按Ctrl+C 停止Server。
> 只有在hexo server启动时，localhost才可用

#### 3.2.3 Create a new post
在/E/Hexo目录下打开git bash窗口，执行下面的命令
```bash
$ hexo new "My New Post"
[info] File created at d:\Hexo\source\_posts\My-New-Post.md
```
刷新http://localhost:4000/ ，可以发现在E:\Hexo\source\_posts已生成了一篇新文章 "My New Post"。

#### 3.2.4 Generate static files
执行下面的命令，将markdown文件生成静态网页。
```bash
$ hexo generate
```
该命令执行完后，会在 D:\Hexo\public\ 目录下生成一系列html，css等文件。

#### 3.2.5 编辑文章
hexo new "My New Post"会在D:\Hexo\source\_posts目录下生成一个markdown文件：My-New-Post.md
可以使用一个支持markdown语法的编辑器（比如 Sublime Text 2）来编辑该文件。
> 只改文件名无效，需要更改文档中的yaml文件头。

#### **3.2.6 <font color=red>部署到Github</font>**

##### 3.2.6-1 部署到Github前需要配置hexo的_config.yml文件，首先找到下面的内容(一般在文档最后)
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
Repository：必须是SSH形式的url（ git@github.com:xiaogliu/xiaogliu.github.io.git ），而不能是HTTPS形式的url（https://github.com/xiaogliu/xiaogliu.github.io.git ），否则会出现错误：
> 此处有坑，有说type为github，但此处应该为git，和hexo版本有关？？？
>
> 另，有说：Be carefull, with your current configuration, you will erase all your source code on master branch. I recommend > you to deploy on `gh-pages` to be automatically reachable at https://github.com/username/repository_name
> deploy:
>    type: git
>    repo: <repository url>
>    branch: gh-pages
> http://stackoverflow.com/questions/34452547/hexoerror-deployer-not-found-github

##### 3.2.6-2 安装hexo-deployer

```bash
npm install hexo-deployer-git --save
```

##### 3.2.6-3 deploy
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

#### 3.2.7 测试
当部署完成后，在浏览器中打开http://xiaogliu.github.io/ ，正常显示网页，表明部署成功。

#### 3.2.8 总结
每次部署的步骤，可按以下三步来进行。
```bash
$ hexo clean
$ hexo generate
$ hexo deploy
```
> 每次写文章/修改文章都要重新全部deploy？？

#### 3.2.9 总结：本地调试
##### 3.2.9-1 在执行下面的命令后，
```bash
$ hexo g #生成
$ hexo s #启动本地服务，进行文章预览调试
```
在浏览器输入http://localhost:4000 ，查看搭建效果。此后的每次变更_config.yml 文件或者新建文件都可以先用此命令调试，尤其是当你想调试新添加的主题时。

##### 3.2.9-2 可以用简化的一条命令
```bash
hexo s -g
```

### 3.3 命令总结

#### 3.3.1 常用命令
```bash
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```

#### 3.3.2 复合命令
```bash
hexo deploy -g  #生成加部署
hexo server -g  #生成加预览
```
#### 3.3.4 命令的简写为
```bash
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

## 四、设计更改
### 4.1 更换主题
在Hexo目录下打开Git Bash，后只需要一句Git命令：

```
git clone https://github.com/heroicyang/hexo-theme-modernist.git themes/modernist
```
> [more theme](https://hexo.io/themes/)

安装完成后，打开hexo_config.yml，修改主题为modernist

```
theme: modernist
```
>打开hexo\themes\modernist目录下的_config.yml，可做有限的定制。若想再次基础上做更多更改，需要编辑主题的js（ejs）、css（styl）等文件。

更新主题：进到主题相应文件夹，pull
```
cd themes/modernist
git pull
```
**添加网站图标**
在hexo\themes\modernist\layout_partial\head.ejs，找到这句：
```
<link rel="icon" type="image/x-icon" href="<%- config.root %>favicon.ico">
```
将你的favicon.ico放到工程根目录下即可，也就是hexo\source目录。

### 4.2 自定义页面
#### 4.2.1 about 页面
执行new page命令
```
hexo new page "about"
```
在hexo\source\下会生成about目录，里面有个index.md，直接编辑就可以了，然后在主题的_config.yml中将其配置显示出来。
上述步骤，也可以手工生成，在hexo\source\下手工新建about和index.md也是完全等价的。

#### 4.2.2 404页面
GitHub Pages自定义[404页面](https://help.github.com/articles/creating-a-custom-404-page-for-your-github-pages-site/)非常容易，直接在根目录下创建自己的404.html就可以。但是自定义404页面仅对绑定顶级域名的项目才起作用，GitHub默认分配的二级域名是不起作用的，使用hexo server在本机调试也是不起作用的。

其实，404页面可以做更多有意义的事，目前有如下几个公益404接入地址:

[腾讯公益404](http://www.qq.com/404/)
[404公益_益云(公益互联网)社会创新中心](http://yibo.iyiyun.com/Index/web404)
[失蹤兒童少年資料管理中心404](http://404page.missingkids.org.tw/)
### 4.3 hexo更多介绍
#### 4.3.1 目录介绍
默认目录结构：
```
.
├── .deploy
├── public
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
├── themes
├── _config.yml
└── package.json
```
- .deploy：执行hexo deploy命令部署到GitHub上的内容目录
- public：执行hexo generate命令，输出的静态网页内容目录
- scaffolds：layout模板文件目录，其中的md文件可以添加编辑
- scripts：扩展脚本目录，这里可以自定义一些javascript脚本
- source：文章源码目录，该目录下的markdown和html文件均会被hexo处理。该页面对应repo的根目录，404文件
- favicon.ico文件，CNAME文件等都应该放这里，该目录下可新建页面目录。
- _drafts：草稿文章
- _posts：发布文章
- themes：主题文件目录
- _config.yml：全局配置文件，大多数的设置都在这里
- package.json：应用程序数据，指明hexo的版本等信息，类似于一般软件中的关于按钮

#### 4.3.2 _config.yml简单说明：
```
# Hexo Configuration
## Docs: http://zespia.tw/hexo/docs/configure.html
## Source: https://github.com/tommy351/hexo/

# Site #整站的基本信息
title: 刘小光 #网站标题
subtitle: 求知，好奇，审美。就自己仅有的才能去发挥，去表达内心深刻的感觉。 #网站副标题
description: #网站描述，给搜索引擎用的，在生成html中的head->meta中可看到
author: 刘小光 #网站作者，在下方显示
email: xiaogliu@outlook.com #联系邮箱
language: zh-CN #语言
timezone:

# URL #域名和文件结构
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://ibruce.info #你的域名
root: /
permalink: :year/:month/:day/:title/
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code

# Writing #写文章选项
new_post_name: :title.md # File name of new posts
default_layout: post #默认layout方式
auto_spacing: false # Add spaces between asian characters and western characters
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
max_open_file: 100
multi_thread: true
filename_case: 0
render_drafts: false
highlight: #代码高亮
  enable: true #是否启用
  line_number: false #是否显示行号
  tab_replace:

# Category & Tag #分类与标签
default_category: uncategorized # default
category_map:
tag_map:

# Archives #存档，这里的说明好像不对。全部选择1，这个选项与主题中的选项有时候会有冲突
## 2: Enable pagination
## 1: Disable pagination
## 0: Fully Disable
archive: 1
category: 1
tag: 1

# Server #本地服务参数
## Hexo uses Connect as a server
## You can customize the logger format as defined in
## http://www.senchalabs.org/connect/logger.html
port: 4000
logger: true
logger_format:

# Date / Time format #日期显示格式
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: MMM D YYYY
time_format: H:mm:ss

# Pagination #分页设置
## Set per_page to 0 to disable pagination
per_page: 10 #每页10篇文章
pagination_dir: page

# Disqus #社会化评论disqus，我使用多说，在主题中配置
disqus_shortname:

# Extensions #插件，暂时未安装插件
## Plugins: https://github.com/tommy351/hexo/wiki/Plugins
## Themes: https://github.com/tommy351/hexo/wiki/Themes
## 主题
theme: modernist # raytaylorism # pacman # modernist # light
exclude_generator:

# Deployment #部署
## Docs: http://zespia.tw/hexo/docs/deploy.html
deploy:
  type: git
  repository: git@github.com:xiaogliu/xiaogliu.github.io.git
  branch: master #你的GitHub Pages仓库
  ```
#### 4.3.3 修改局部页面
页面展现的全部逻辑都在每个主题中控制，源代码在hexo\themes\你使用的主题\中，以modernist主题为例：
```
.
├── languages          #多语言
|   ├── default.yml    #默认语言
|   └── zh-CN.yml      #中文语言
├── layout             #布局，根目录下的*.ejs文件是对主页，分页，存档等的控制
|   ├── _partial       #局部的布局，此目录下的*.ejs是对头尾等局部的控制
|   └── _widget        #小挂件的布局，页面下方小挂件的控制
├── source             #源码
|   ├── css            #css源码
|   |   ├── _base      #*.styl基础css
|   |   ├── _partial   #*.styl局部css
|   |   ├── fonts      #字体
|   |   ├── images     #图片
|   |   └── style.styl #*.styl引入需要的css源码
|   ├── fancybox       #fancybox效果源码
|   └── js             #javascript源代码
├── _config.yml        #主题配置文件
└── README.md          #用GitHub的都知道
```
如果你需要修改头部，直接修改hexo\themes\modernist\layout_partial\header.ejs，比如头上加个搜索框：
```
<div>
<form class="search" action="//google.com/search" method="get" accept-charset="utf-8">
 <input type="search" name="q" id="search" autocomplete="off" autocorrect="off" autocapitalize="off" maxlength="20" placeholder="Search" />
 <input type="hidden" name="q" value="site:<%- config.url.replace(/^https?:\/\//, '') %>">
</form>
</div>
```
将如上代码加入即可，您需要修改css以便这个搜索框比较美观。

再如，你要修改页脚版权信息，直接编辑hexo\themes\modernist\layout_partial\footer.ejs。同理，你需要修改css，直接去修改对应位置的styl文件。

## 五、多台电脑协同维护blog
这就需要一开始配置github仓库时做好：第一次搭建blog时git新建两个分支，一个master，发布更新，一个source（或其他任意名称）进行维护(放置所有原始markdown文件)。每次更新blog，都是再source分支更新，然后deploy到master。

>我第一次搭建blog时，发布的分支和维护的分支没有区分，虽然建了两个分支，但并不清楚原理，时隔几个月再看时，发现维护的source分支并没有及时推送到远程仓库，结果，拉回source分支一看，markdown文件都丢失了，这样发布master后，之前的文件就都没有了！

- 日常维护流程：
 1、在source分支添加、修改blog内容，修改完后git add , commit, push；
 2、执行hexo g -d，发布到master（_config.yml中的deploy参数，分支已经设置为master）

- 本地资料丢失（比如重装电脑或者更换电脑）后进行下列步骤维护：
 1、将source分支拉到本地
 ```
 git clone -b source https://github.com/xiaogliu/xiaogliu.github.io
 ```
 > 之所以日常维护每次更改markdown都要push，实际也是备份的过程，不然拉回的是不完整的blog
 2、进入hexo文件夹，执行
 ```
 npm install
 ```
 > 实际不需要重新pm install hexo、npm install、npm install hexo-deployer-git，因为执行npm install，hexo,hexo-deployer-git就已经安装，因为package.json文件里都写了相应依赖了，如下
 ```
 {
   "name": "hexo-site",
   "version": "0.0.0",
   "private": true,
   "hexo": {
     "version": "3.2.2"
   },
   "dependencies": {
     "hexo": "^3.2.0",
     "hexo-deployer-git": "^0.1.0",
     "hexo-generator-archive": "^0.1.4",
     "hexo-generator-category": "^0.1.3",
     "hexo-generator-index": "^0.2.0",
     "hexo-generator-tag": "^0.2.0",
     "hexo-renderer-ejs": "^0.2.0",
     "hexo-renderer-marked": "^0.2.10",
     "hexo-renderer-stylus": "^0.3.1",
     "hexo-server": "^0.2.0"
   }
 }
 ```

## 六、遗留问题
能否在同一个分支直接维护？毕竟hexo在public文件夹下面已经有所需要的html文件了

### 参考资料
【1】[hexo你的博客](http://ibruce.info/2013/11/22/hexo-your-blog/)
【2】[Hexo搭建Github静态博客](http://www.cnblogs.com/zhcncn/p/4097881.html")
【3】[使用hexo，如果换了电脑怎么更新博客？CrazyMilk回答](https://www.zhihu.com/question/21193762)
【4】[SSH-互动百科](http://www.baike.com/wiki/ssh)
【5】[Secure Shell-维基百科](https://en.wikipedia.org/wiki/Secure_Shell)

