---
title: 通过JSONP和CORS实现跨域请求
date: 2017-10-18 15:06:32
tags: [JavaScript]
categories: JavaScript
e_title: cross-origin-request-using-jsonp-and-cors
---

# 为什么会产生跨域

跨域产生的场景主要是在：如果A页面（通过B页面提供的API）获取B页面的资源，而A页面和B页面的源信息不同，那么便会产生跨域错误。  

> 源信息包括：协议、域名、端口三部分，同源指的是源信息的协议、域名、端口都相同

这主要源于 [跨域安全策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)，简单说，如果两个页面的源信息不同，就不能通过 [XHR](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 对象获取对方的信息。

# 跨域场景

- 说明

url  | 说明  | 是否允许通信
--|---|--
http://www.domain.com/a.js <br> http://www.domain.com/b.js <br> http://www.domain.com/c/c.js  | 同源，不同文件或路径  |  允许
http://www.domain.com:8080/a.js <br> http://www.domain.com/b.js  | 协议、域名相同，端口不同 | 不允许
http://www.domain.com/a.js <br> http://www.domain111.com/b.js  | 协议、端口相同，域名不同 | 不允许
http://www.domain.com/a.js <br> http://x.domain.com/b.js <br> http://domain.com/c.js | 协议、端口相同，域名不同（主域相同，子域不同） | 不允许
http://www.domain.com/a.js <br> http://127.0.0.1/b.js | 协议、端口相同，域名不同（即便 www.domain.com 对应的ip地址为 127.0.0.1） | 不允许
http://www.domain.com/a.js <br> https://www.domain.com/b.js | 域名、端口相同，协议不同 | 不允许

- 实例

下面是通过写原生 XHR 对象，调用豆瓣图书的API

```js
const xhr = new XMLHttpRequest();

xhr.onreadystatechange = function () {
  if (xhr.readyState === 4) {
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
      console.log(xhr.responseText);
    } else {
      console.log("Error" + xhr.status);
    }
  }
};
// 获取id为6548683的图书信息
xhr.open('get', 'https://api.douban.com/v2/book/6548683', true);
xhr.send(null);
```

直接使用上述代码会报下面的错误：

```
Failed to load https://api.douban.com/v2/book/6548683:
No 'Access-Control-Allow-Origin' header is present on the requested resource.
Origin 'http://127.0.0.1:7070' is therefore not allowed access.
```

该错误说明 `https://api.douban.com` 在响应头信息中没有设置 `Access-Control-Allow-Origin` （CORS标准中定义的内容，后面讨论），这就不允许跨域请求。
