---
title: Atom配置
date: 2017-08-27 13:15:24
tags: [atom]
categories: 编程工具
e_title: atom-configuration
---

# 编辑器配置

## Non Word Charactors

```
/\()"':,.;<>~!@#$%^&*|+=[]{}`?…
```

> 双击选择文本时中划线`-`作为单词的一部分，不作为分割依据

## Preferred Line Length

```
1000
```

> 不一定是200，主要不想看到编辑器中的分割线，大于编辑器能显示的最大字符就好

## Show Indent Guide

复选框打钩，可以显示对齐线。   

# 快捷键

## 自定义快捷键配置如下：

```atom
'atom-text-editor':
  'ctrl-alt-i':       'editor:auto-indent'
  'ctrl-d':           'editor:delete-line'
  'ctrl-y':           'editor:duplicate-lines'
  'alt-j':            'find-and-replace:select-next'
  'shift-alt-f':      'project-find:show'
  'ctrl-shift-alt-m': 'markdown-preview:toggle'
```

## 系统默认常用快捷键

操作                       | 快捷键
---------------------------|-----------------
toggle目录树               | ctrl + \
在当前行的下一行插入一空行 | ctrl + enter
在项目中搜索文件           | ctrl + p
调出命令窗口               | ctrl + shift + p

> 显示隐藏不可见字符操作：首先`ctrl + shift + p`调出命令行，然后在搜索框输入`invisible`，选中`Window Toggle Invisibles`。这在确认编辑器中是否混用`tab`，`space`，或者查看行结尾是Unix类型还是Windows类型时很便捷，不用去设置里设置。

# 插件

## 必备

[sublime-style-column-selection](https://atom.io/packages/Sublime-Style-Column-Selection)：支持列选择；   
[atom-terminal](https://atom.io/packages/atom-terminal)：在当前目录打开终端；   
[highlight-selected](https://atom.io/packages/highlight-selected)：自动高亮选中的相同字符；   
[minimap](https://atom.io/packages/minimap)：代码缩略地图；   

## 语言相关

[emmet](https://atom.io/packages/emmet)：编写html神奇，强大的自动补全；   
[markdown-writer](https://atom.io/packages/markdown-writer)：书写markdown利器，支持各种快捷键；   
[language-vue](https://atom.io/packages/language-vue)：Vue语法高亮；   
[pigments](https://atom.io/packages/pigments)：CSS颜色高亮；   
[atom-typescript](https://atom.io/packages/atom-typescript)：typescript颜色高亮；   

# 主题

**UI Theme**： one dark;
**Syntax Theme**: Monokai。

> [Monokai](https://atom.io/themes/monokai)主题需要单独安装

# 其他

## windows下默认安装失败

如果在windows下默认安装失败，提示`Path too long exception`，下载安装包后参考下面解决方式：   

1. 用解压文件打开AtomSetup.exe；
2. 将文件内容导出到相对较短的路径中，比如`C:\Temp`；
3. 打开CMD，进入刚才放Atom导出文件的文件目录；
4. 运行`Update.exe --install=.` 。

# 参考资料
【1】[Path too long exception on Windows install](https://github.com/atom/atom/issues/5109)   
【2】[Atom编辑器中的自动缩进代码](https://gxnotes.com/article/71037.html)   
