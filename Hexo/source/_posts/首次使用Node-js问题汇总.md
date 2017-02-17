---
title: 首次使用Node.js问题汇总
date: 2017-02-16 18:39:02
tags: [Node.js,小白之路]
categories: 编程语言
comments: false
---

由于项目原因，首次接触Node，遇到好多问题，在此做下记录。

项目中之所以引入Node是之前的后端接口都是直接暴露给用户，安全性差，且后端开发的API高度定制，复用性差。为了提升项目安全性，同时使后端开发的接口可以更多的复用，页面和底层接口之间加多Node中间层：页面直接调用Node中间层提供的API，经过Node处理后，再调后端接口。

顺便说一句：Node提供的API由前端编写，这样不管是前期开发量还是后期的调试，前端工作量增大很多，不止停留在页面部分，而后端更专注于基础功能。

<!-- more -->

### 项目中引入Node

略，后续补充。

### 如何在本地查看Node返回的错误日志

之前前端调试页面都直接通过浏览器调试，后台API出问题可以直接看到，加多Node中间层后，API的错误信息不能在浏览器中直接查看，需要查看Node返回的log信息。

比如， `url` 设置错误，报如下错误：
```
{
  "message": "Error: getaddrinfo ENOTFOUND 172.31.75.158undefined 172.31.75.158undefined:80",
  "name": "RequestError",
  "cause": {
    "code": "ENOTFOUND",
    "errno": "ENOTFOUND",
    "syscall": "getaddrinfo",
    "hostname": "172.31.75.158undefined",
    "host": "172.31.75.158undefined",
    "port": 80
  },
  "error": {
    "code": "ENOTFOUND",
    "errno": "ENOTFOUND",
    "syscall": "getaddrinfo",
    "hostname": "172.31.75.158undefined",
    "host": "172.31.75.158undefined",
    "port": 80
  },
  "options": {
    "method": "POST",
    "uri": "http://172.31.75.158undefined",
    "body": {
      "serviceId": "usersystem/company/firstRegisterAdmin/v1",
      "companyCode": "28321636",
      "token": "8e6062e21b3e90647fe65e575c702f14",
      "secretNumber": "3d406ac04e2a193276f6d09ba56ba2b9",
      "commonParam": {
        "sourceRoute": "/commons/invite/company/:companyId"
      },
      "ipAddress": "127.0.0.1"
    },
    "json": true,
    "simple": true,
    "resolveWithFullResponse": false,
    "transform2xxOnly": false
  },
  "stack": "RequestError: Error: getaddrinfo ENOTFOUND 172.31.75.158undefined 172.31.75.158undefined:80\n    at new RequestError (X:\\zhilink\\project\\feature_node\\node_modules\\.1.1.1@request-promise-core\\lib\\errors.js:14:15)\n    at Request.plumbing.callback (X:\\zhilink\\project\\feature_node\\node_modules\\.1.1.1@request-promise-core\\lib\\plumbing.js:87:29)\n    at Request.RP$callback [as _callback] (X:\\zhilink\\project\\feature_node\\node_modules\\.1.1.1@request-promise-core\\lib\\plumbing.js:46:31)\n    at self.callback (X:\\zhilink\\project\\feature_node\\node_modules\\.2.79.0@request\\request.js:186:22)\n    at emitOne (events.js:77:13)\n    at Request.emit (events.js:169:7)\n    at Request.onRequestError (X:\\zhilink\\project\\feature_node\\node_modules\\.2.79.0@request\\request.js:845:8)\n    at emitOne (events.js:77:13)\n    at ClientRequest.emit (events.js:169:7)\n    at Socket.socketErrorListener (_http_client.js:269:9)\n    at emitOne (events.js:77:13)\n    at Socket.emit (events.js:169:7)\n    at connectErrorNT (net.js:1012:8)\n    at nextTickCallbackWith2Args (node.js:442:9)\n    at process._tickCallback (node.js:356:17)",
  "level": "error",
  "timestamp": "2017-02-16 16:54:04"
}
```
其中message是请求返回的信息，通常为 `json` 字符串，此处关键字信息为 `172.31.75.158undefined` ，那就查下哪里导致url错误。

又如，
```
{
  "message": "404 - \"<html>\\r\\n<head><title>404 Not Found</title></head>\\r\\n<body bgcolor=\\\"white\\\">\\r\\n<center><h1>404 Not Found</h1></center>\\r\\n<hr><center>openresty/1.9.7.3</center>\\r\\n</body>\\r\\n</html>\\r\\n\"",
  "name": "StatusCodeError",
  "statusCode": 404,
  "error": "<html>\r\n<head><title>404 Not Found</title></head>\r\n<body bgcolor=\"white\">\r\n<center><h1>404 Not Found</h1></center>\r\n<hr><center>openresty/1.9.7.3</center>\r\n</body>\r\n</html>\r\n",
  "options": {
    "method": "POST",
    "uri": "http://172.31.75.89/usersystem/company/firstRegisterAdmin/v1",
    "body": {
      "companyCode": 28321636,
      "token": "8e6062e21b3e90647fe65e575c702f14",
      "secretNumber": "3d406ac04e2a193276f6d09ba56ba2b9",
      "commonParam": {
        "sourceRoute": "/commons/invite/company/:companyId"
      },
      "ipAddress": "127.0.0.1"
    },
    "json": true,
    "simple": true,
    "resolveWithFullResponse": false,
    "transform2xxOnly": false
  },
  "response": "<html>\r\n<head><title>404 Not Found</title></head>\r\n<body bgcolor=\"white\">\r\n<center><h1>404 Not Found</h1></center>\r\n<hr><center>openresty/1.9.7.3</center>\r\n</body>\r\n</html>\r\n",
  "stack": "StatusCodeError: 404 - \"<html>\\r\\n<head><title>404 Not Found</title></head>\\r\\n<body bgcolor=\\\"white\\\">\\r\\n<center><h1>404 Not Found</h1></center>\\r\\n<hr><center>openresty/1.9.7.3</center>\\r\\n</body>\\r\\n</html>\\r\\n\"\n    at new StatusCodeError (X:\\zhilink\\project\\feature_node\\node_modules\\.1.1.1@request-promise-core\\lib\\errors.js:32:15)\n    at X:\\zhilink\\project\\feature_node\\node_modules\\.1.1.1@request-promise-core\\lib\\plumbing.js:97:41\n    at tryCatcher (X:\\zhilink\\project\\feature_node\\node_modules\\.3.4.7@bluebird\\js\\release\\util.js:16:23)\n    at Promise._settlePromiseFromHandler (X:\\zhilink\\project\\feature_node\\node_modules\\.3.4.7@bluebird\\js\\release\\promise.js:510:31)\n    at Promise._settlePromise (X:\\zhilink\\project\\feature_node\\node_modules\\.3.4.7@bluebird\\js\\release\\promise.js:567:18)\n    at Promise._settlePromiseCtx (X:\\zhilink\\project\\feature_node\\node_modules\\.3.4.7@bluebird\\js\\release\\promise.js:604:10)\n    at Async._drainQueue (X:\\zhilink\\project\\feature_node\\node_modules\\.3.4.7@bluebird\\js\\release\\async.js:138:12)\n    at Async._drainQueues (X:\\zhilink\\project\\feature_node\\node_modules\\.3.4.7@bluebird\\js\\release\\async.js:143:10)\n    at Immediate.Async.drainQueues [as _onImmediate] (X:\\zhilink\\project\\feature_node\\node_modules\\.3.4.7@bluebird\\js\\release\\async.js:17:14)\n    at processImmediate [as _immediateCallback] (timers.js:383:17)",
  "level": "error",
  "timestamp": "2017-02-16 18:22:45"
}
```
message为 `"message":"404 - \"<html>` 这里是404错误，请求url本身有问题，和后台沟通。
