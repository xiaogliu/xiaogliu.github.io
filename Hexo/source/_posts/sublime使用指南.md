---
title: sublime使用指南
date: 2017-03-17 21:34:16
tags: [编辑器,sublime]
categories: 编程工具
comments: false
---
# 安装插件（包括主题）
- 方法一：使用package control
 `ctrl+shift+p` 调出命令面板，输入install根据提示找到`package control：install package` ，选中并回车，输入插件名安装，页面左下角会提示安装进度。

  > 有文章说需要先调出console，输入下面代码才可以使用package control，目前（2017/3/17）我下载sublime3，package control已经安装好，无需这步操作（如果在menu > preferences下面有package control，证明已安装好）
  ```
  import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
 ```   
- 方法二： 直接将package文件夹放到sublime package目录（menu > preferences > browse packages，点击直接打开package文件夹）
 > 若国内网络环境不允许或者，更换电脑，用此方法方便快捷，所以记得备份自己的package   

# 删除插件
`ctrl+shift+p` 调出命令面板，输入remove根据提示找到 `package control：remove package`，直接输入需要删除的插件名称，或者用上下方向键选择需要删除的插件。

# 义快捷键
## 常用默认快捷键：   
`ctrl + p`: 在项目中快速查找文件   
## 自定义常用快捷键：   
menu > preferences > key bindings,在user文件中，输入自定义快捷键。自己自定以快捷键如下：
```
[
	{ "keys": ["ctrl+alt+i"], "command": "reindent" },
	{ "keys": ["alt+1"], "command": "toggle_side_bar" },
	{ "keys": ["alt+2"], "command": "show_panel", "args": {"panel": "console", "toggle": true} },
	{ "keys": ["ctrl+d"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Delete Line.sublime-macro"} },
	{ "keys": ["ctrl+y"], "command": "swap_line_down" },
	{ "keys": ["ctrl+o"], "command": "run_macro_file", "args": {"file": "res://Packages/Default/Add Line.sublime-macro"} },
	{ "keys": ["ctrl+shift+-"], "command": "fold" },
	{ "keys": ["ctrl+shift+="], "command": "unfold" },
	{ "keys": ["alt+shift+f"], "command": "show_panel", "args": {"panel": "find_in_files"} },
	{ "keys": ["alt+j"], "command": "find_under_expand" },
]
```
# 自用插件
- Package Control：插件管理，已随sublime3安装好；
- [Emmet](https://github.com/sergeche/emmet-sublime)：html编辑自动补全；
- [Pretty Json](https://github.com/dzhibas/SublimePrettyJson)：json格式化；
- [BracketHightLighter](https://github.com/facelessuser/BracketHighlighter)：成对括号、标签高亮（自定义颜色高亮点这里[Sublime Text3 BracketHighlighter高亮色彩配置](http://www.cnblogs.com/willingtolove/p/4657320.html)）；
- [SideBarEnhancements](https://github.com/titoBouzout/SideBarEnhancements)：在sidebar中增强右键功能，比如在浏览器中打开文件；
- [JsFormat](https://github.com/jdc0589/JsFormat)：格式化js代码；
- [TrailingSpaces](https://github.com/SublimeText/TrailingSpaces)：高亮多余空格或tab；
- [CSScomb](https://github.com/csscomb/CSScomb-for-Sublime)：css属性排序；
- [AutoFileName](https://github.com/BoundInCode/AutoFileName)：自动补全文件路径，比如图片
- [GitGutter](https://github.com/jisaacks/GitGutter)：配合git显示文件增减标志
- [Minify](https://github.com/tssajo/Minify)：压缩js,css文件（和Minifier不同）
- [Theme-soda](https://github.com/buymeasoda/soda-theme)：清爽不浮夸

# 参考资料
【1】[那些年我使用过的 Sublime Text 3 插件](http://bubkoo.com/2014/01/04/sublime-text-3-plugins/)   
【2】[如何优雅地使用Sublime Text](http://jeffjade.com/2015/12/15/2015-04-17-toss-sublime-text/)   
【3】[Sublime Text3 BracketHighlighter高亮色彩配置](http://www.cnblogs.com/willingtolove/p/4657320.html)
