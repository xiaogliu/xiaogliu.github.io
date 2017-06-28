---
title: Gulp安装及使用
date: 2017-05-10 21:33:34
tags: [Gulp,Babel]
categories: 编程工具
---

# Gulp用途

Gulp是自动任务执行工具，主要用途是把开发者从重复劳动中解放出来，Gulp可以做但不限于以下工作：

- 转换、压缩、合并js或者css文件（包括Less/Sass文件转CSS文件，ECMA-262高版本转低版本）
- 设置某次改动后自动更新版本号、提交commit到git等
- 在编辑器中点击保存按钮自动刷新浏览器页面
- 运行代码分析
- 快速运行单元测试

其中压缩、转换、合并js或者css文件是最为常用功能。

> Gulp借鉴了Unix操作系统的管道（pipe）思想，很多人认为，在操作上，它要比Grunt简单

# Gulp 安装

如果之前全局安装过Gulp，先卸载全局安装的Gulp`npm rm --global gulp`，然后开始下面的步骤。

> Gulp维护团队已不再建议全局安装`gulp`，可参参考这个issue[gulp vs gulp-cli reasoning unclear in getting started guide](https://github.com/gulpjs/gulp/issues/1795)，但在[gulp中文网](http://www.gulpjs.com.cn/docs/getting-started/) 还未更新，依旧建议全局安装`gulp`，安装时请注意。

**步骤一**，全局安装Gulp命令行工具   

任意目录执行下面命令：

```bash
$ npm install --global gulp-cli
```

> 全局安装不是必须的，如果只是在项目中引入Gulp而不希望全局安装Gulp命令行工具，可以通过**npm script**实现对Gulp的调用。

**步骤二**，项目本地安装Gulp   

项目根目录执行下面命令：

```bash
$ npm install --save-dev gulp
```

**步骤三**，常见`gulpfile.js`文件

在项目根目录新建`gulpfile.js`文件，并添加下面内容：

```js
var gulp = require('gulp');

gulp.task('default', function() {
  // place code for your default task here
});
```

> `default`是输入`gulp`以后默认执行的任务，如果要执行过个任务，可以用这个命令`gulp <task> <othertask>`

**步骤四**，测试是否成功

输入`gulp`命令，如果输出以下打印内容，说明Gulp安装成功：

```bash
$ gulp
[16:22:02] Using gulpfile x:\study\myProject\calculator_for_house_loan\jquery\gulpfile.js
[16:22:02] Starting 'default'...
[16:22:02] Finished 'default' after 63 μs
```

# Gulp配置

Gulp强大之处在于可以引入插件实现一系列功能。下面是引入压缩js文件插件的例子：

```js
var gulp = require('gulp');
var uglify = require('gulp-uglify');

gulp.task('minify', function () {
  gulp.src('js/app.js')
    .pipe(uglify())
    .pipe(gulp.dest('build'))
});
```

**注意：** 如果使用ES6语法，`gulp-uglify`会报错`GulpUglifyError: unable to minify JavaScript`，需要引入Babel对ES6语法进行转换，更改后代码如下（同时引入错误标记插件`gulp-util`）：

```js
// 引入gulp
var gulp = require('gulp');

// 引入依赖模块
var babel = require("gulp-babel");
var uglify = require('gulp-uglify');
var gutil = require('gulp-util'); // 错误标记插件

gulp.task('default', function () {
  return gulp.src('js/main_js.js') // 加return，finish后马上结束，但显示总时间长
      .pipe(babel())
      .pipe(uglify())
      .on('error', function (err) { gutil.log(gutil.colors.red('[Error]'), err.toString()); })
      .pipe(gulp.dest('build'))
});
```

> `gulp.task`, `gulp.src`, `gulp.dest`等属于Gulp的API，多更信息可参看[gulp API docs](https://github.com/gulpjs/gulp/blob/master/docs/API.md)

# 参考资料
【1】 [gulp documentation](https://github.com/gulpjs/gulp/blob/master/docs/README.md)   
【2】 [What is gulp.js and why use it?](http://brandonclapp.com/what-is-gulp-js-and-why-use-it/)    
【3】 [Gulp：任务自动管理工具](http://javascript.ruanyifeng.com/tool/gulp.html#comment-3299560540)



