---
title: sublime使用指南
date: 2017-03-17 21:34:16
tags: [编辑器,sublime]
categories: 编程工具
---

# 插件管理

## 安装插件（包括主题）   
- 方法一：使用package control    
  `ctrl+shift+p` 调出命令面板，输入install根据提示找到`package control：install package` ，选中并回车，输入插件名安装，页面左下角会提示安装进度。

  > 有文章说需要先调出console，输入下面代码才可以使用package control，目前（2017/3/17）我下载sublime3，package control已经安装好，无需这步操作（如果在menu > preferences下面有package control，证明已安装好）   
  ```
  import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
  ```
- 方法： 直接将package文件夹放到sublime package目录（menu > preferences > browse packages，点击直接打开package文件夹）   

  > 若国内网络环境不允许或者，更换电脑，用此方法方便快捷

## 删除插件
`ctrl+shift+p` 调出命令面板，输入remove根据提示找到 `package control：remove package`，直接输入需要删除的插件名称，或者用上下方向键选择需要删除的插件。

## 其他插件管理选项
- 查看插件： list package
- 更新插件： upgrade package   
- 禁用插件： disable package   
- 激活插件： enable package   

> 禁用插件等同于在preferences > settings > user里添加需要忽略的插件：
```js
  "ignored_packages":
  [
    "Vintage" // 在这个数组里面添加需要忽略的插件
  ],
```

## 自用插件
- Package Control：插件管理，已随sublime3安装好；
- [AutoFileName](https://github.com/BoundInCode/AutoFileName)：自动补全文件路径，比如图片；
- [Babel](https://github.com/babel/babel-sublime)：将ES6语法转化为ES5（2017-05-31更）；
- [BracketHightLighter](https://github.com/facelessuser/BracketHighlighter)：成对括号、标签高亮（自定义颜色高亮点这里[优化BracketHighlighter高亮显示](http://xiaogliu.github.io/2017/04/07/sublime%E8%87%AA%E5%AE%9A%E4%B9%89%E9%A2%9C%E8%89%B2/#优化BracketHighlighter高亮显示)）；
- [CSScomb](https://github.com/csscomb/CSScomb-for-Sublime)：css属性排序；
- [Emmet](https://github.com/sergeche/emmet-sublime)：html编辑自动补全；
- [JSHint Gutter](https://github.com/victorporof/Sublime-JSHint)：js代码检查工具；
- [Markdown Extended](https://github.com/jonschlinkert/sublime-markdown-extended)：markdown高亮优化（查看[效果图](http://xiaogliu.github.io/2017/04/07/sublime%E8%87%AA%E5%AE%9A%E4%B9%89%E9%A2%9C%E8%89%B2/#优化markdown高亮显示)）；
- [Markdown Preview](https://github.com/revolunet/sublimetext-markdown-preview)：Markdown预览插件；
- [Markdown Table Formatter](https://github.com/bitwiser73/MarkdownTableFormatter)：格式化表格；
- [Markdown Editing](https://github.com/SublimeText-Markdown/MarkdownEditing)：加粗、斜体可以使用快捷键；
- [SideBarEnhancements](https://github.com/titoBouzout/SideBarEnhancements)：在sidebar中增强右键功能，比如在浏览器中打开文件；
- [Terminal](https://packagecontrol.io/packages/Terminal)：在sublime使用过程中打开终端；
- [Theme-soda](https://github.com/buymeasoda/soda-theme)：清爽不浮夸；
- [Vintageous](https://github.com/guillermooo/Vintageous)：在sublime下开启vi/vim模式。

下面的可选：

- [Color Highlighter](https://github.com/Monnoroch/ColorHighlighter)：配合git显示文件增减标志；（影响性能）
- [GitGutter](https://github.com/jisaacks/GitGutter)：配合git显示文件增减标志；（影响性能）
- [JsFormat](https://github.com/jdc0589/JsFormat)：格式化js代码；（手写注意为主，代码检查为辅）
- [Minify](https://github.com/tssajo/Minify)：压缩js,css文件（和Minifier不同）；
- [Pretty Json](https://github.com/dzhibas/SublimePrettyJson)：json格式化；（一般js文件在Chrome插件[JSON-handle](https://chrome.google.com/webstore/detail/json-handle/iahnhfdhidomcpggpaimmmahffihkfnj?utm_source=chrome-app-launcher-info-dialog)中看，日志类长文件在Notepad++中看）
- [TrailingSpaces](https://github.com/SublimeText/TrailingSpaces)：高亮多余空格或tab；（手写注意为主，代码检查为辅）

> sublime归根结底是文本编辑器，如果安装太多对代码进行大量渲染的插件，比如`GitGutter`, `Color Highlighter`，对性能肯定有所影响。当文件较小时这种影响不是很明显，但当文件大了以后，比如代码超过10000行，这种影响是很明显的。

# 常用快捷键

## 常用默认快捷键：   
`ctrl + p`: 在项目中快速查找文件   
>方向键下进行选择，按`enter`打开单个文件，**按`右方向键`可以打开多个文件**   

## 自定义常用快捷键：   
menu > preferences > key bindings,在user文件中，输入自定义快捷键。自己自定以快捷键如下：   

```
[
  { "keys": ["ctrl+alt+i"], "command": "reindent" },
  { "keys": ["alt+1"], "command": "toggle_side_bar" },
  { "keys": ["alt+2"],"command": "open_terminal"},
  { "keys": ["alt+3"], "command": "show_panel", "args": {"panel": "console", "toggle": true} },
  { "keys": ["ctrl+d"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Delete Line.sublime-macro"} },
  { "keys": ["ctrl+y"], "command": "duplicate_line" },
  { "keys": ["ctrl+o"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Add Line.sublime-macro"} },
  { "keys": ["ctrl+shift+-"], "command": "fold" },
  { "keys": ["ctrl+shift+="], "command": "unfold" },
  { "keys": ["alt+shift+f"], "command": "show_panel", "args": {"panel": "find_in_files"} },
  { "keys": ["alt+j"], "command": "find_under_expand" },
  {
    "keys": ["j", "j"],
    "command": "_enter_normal_mode",
    "args": {
      "mode": "mode_insert"
    },
    "context": [{"key": "vi_insert_mode_aware"}]
  }
]
```

# 其他设置
以下在menu > preferences > settings > user中设置

## 选中带中划线“-”的文本（如css的class名）
```
word_separators": "./\\()\"':,.;<>~!@#$%^&*|+=[]{}`~?",
```
## 显示space，tab符号
```
"draw_white_space": "all",
```
## 换行为UNIX格式（LF）
```
"default_line_ending": "unix",
```

> 换行符详细区分见[回车和换行](http://www.ruanyifeng.com/blog/2006/04/post_213.html)   

## user文件所有设置如下   
```json
{
  "color_scheme": "Packages/User/SublimeLinter/Monokai Soda (SL).tmTheme",
  "default_line_ending": "unix",
  "draw_white_space": "all",
  // "font_face": "YaHei Consolas Hybrid",
  // "font_size": 11,
  "font_face": "Ubuntu Mono",
  "font_size": 12,
  "ignored_packages":
  [
    "Vintage"
  ],
  "soda_classic_tabs": true,
  "tab_size": 2,
  "theme": "Soda Dark 3.sublime-theme",
  "word_separators": "./\\()\"':,.;<>~!@#$%^&*|+=[]{}`~?"
}
```

> 需要在系统中安装Ubuntu Mono字体，下载地址[Ubuntu Font](http://font.ubuntu.com/)。

# 参考资料
【1】[那些年我使用过的 Sublime Text 3 插件](http://bubkoo.com/2014/01/04/sublime-text-3-plugins/)   
【2】[如何优雅地使用Sublime Text](http://jeffjade.com/2015/12/15/2015-04-17-toss-sublime-text/)   
【3】[Sublime Text3 BracketHighlighter高亮色彩配置](http://www.cnblogs.com/willingtolove/p/4657320.html)   
【4】[sublime text3 双击选中带中划线-的文本](http://www.css88.com/archives/4539)
