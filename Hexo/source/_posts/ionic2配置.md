---
title: ionic2配置
date: 2017-02-18 00:30:46
tags: [ionic2,AngularJS]
categories: 编程语言
---
## 问题汇总
- ionic安装超级慢
 解决方法：使用淘宝镜像 `cnpm` ，或者 `npm set registry https://registry.npm.taobao.org`

- 安装过程部分node-modules安装慢
 解决方法：使用 `cnpm` 进行安装

- 执行 `ionic start --v2 myApp tabs` 报错
 ```
 Downloading binary from https://github.com/sass/node-sass/releases/downlo
 ad/v4.5.0/win32-x64-46_binding.node
 Cannot download "https://github.com/sass/node-sass/releases/download/v4.5
 .0/win32-x64-46_binding.node":

 ESOCKETTIMEDOUT

 Hint: If github.com is not accessible in your location
       try setting a proxy via HTTP_PROXY, e.g.

       export HTTP_PROXY=http://example.com:1234

 or configure npm proxy via

       npm config set proxy http://example.com:8080

 Binary found at X:\study\ionic\test\myApp\node_modules\@ionic\app-scripts
 \node_modules\node-sass\vendor\win32-x64-46\binding.node
 Testing binary
 Binary has a problem: [Error: %1 is not a valid Win32 application.
 \\?\X:\study\ionic\test\myApp\node_modules\@ionic\app-scripts\node_module
 s\node-sass\vendor\win32-x64-46\binding.node]‘【】
 ```
 > 在公司也遇到有些modules不能安装的情况，node版本问题？？？

- 错误信息
 ```
 Error: Cannot find module '@angular/tsc-wrapped/src/tsc'
     at Function.Module._resolveFilename (module.js:325:15)
     at Function.Module._load (module.js:276:25)
     at Module.require (module.js:353:17)
     at require (internal/module.js:12:17)
     at Object.<anonymous> (X:\study\ionic\test\myApp\node_modules\.1.1.0@@io
 nic\app-scripts\dist\aot\aot-compiler.js:6:13)
     at Module._compile (module.js:409:26)
     at Object.Module._extensions..js (module.js:416:10)
     at Module.load (module.js:343:32)
     at Function.Module._load (module.js:300:12)
     at Module.require (module.js:353:17)
 ```
 直接执行 `cnpm install @angular/tsc-wrapped/src/tsc` 又报错
 ```
 Install fail! Error: [@@angular/tsc-wrapped/src/tsc] resolved target X:\s
 tudy\ionic\test\myApp\@angular\tsc-wrapped\src\tsc error: ENOENT: no such fi
 le or directory, lstat 'X:\study\ionic\test\myApp\@angular'
 ```

 解决方法：升级npm？
 ```
 @iaashish i was facing issue - tried following and it worked
 reinstalled npm to latest version
 npm uninstall -g angular-cli
 npm cache clean
 npm install -g angular-cli@latest
 ```