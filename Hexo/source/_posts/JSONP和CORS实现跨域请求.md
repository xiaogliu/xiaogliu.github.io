---
title: JSONP和CORS实现跨源请求
date: 2017-10-18 15:06:32
tags: [JavaScript]
categories: JavaScript
e_title: cross-origin-request-using-jsonp-and-cors
---

# 为什么会产生跨源

跨源产生的场景主要是在：如果A页面（通过B页面提供的API）获取B页面的资源，而A页面和B页面的源信息不同，那么便会产生跨源错误。  

> 源信息包括：协议、域名、端口三部分，同源指的是源信息的协议、域名、端口都相同

这主要源于 [跨源安全策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)，简单说，如果两个页面的源信息不同，就不能通过 [XHR](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 对象获取对方的信息。

# 跨源场景

## 跨源场景说明

url  | 说明  | 是否允许通信
--|---|--
http://www.domain.com/a.js <br> http://www.domain.com/b.js <br> http://www.domain.com/c/c.js  | 同源，不同文件或路径  |  允许
http://www.domain.com:8080/a.js <br> http://www.domain.com/b.js  | 协议、域名相同，端口不同 | 不允许
http://www.domain.com/a.js <br> http://www.domain111.com/b.js  | 协议、端口相同，域名不同 | 不允许
http://www.domain.com/a.js <br> http://x.domain.com/b.js <br> http://domain.com/c.js | 协议、端口相同，域名不同（主域相同，子域不同） | 不允许
http://www.domain.com/a.js <br> http://127.0.0.1/b.js | 协议、端口相同，域名不同（即便 www.domain.com 对应的ip地址为 127.0.0.1） | 不允许
http://www.domain.com/a.js <br> https://www.domain.com/b.js | 域名、端口相同，协议不同 | 不允许

## 跨源实例

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

该错误说明 `https://api.douban.com` 在响应头信息中没有设置 `Access-Control-Allow-Origin` （CORS标准中定义的内容，后面讨论），这就不允许跨源请求。

> 此处我通过Node开启了本地服务，通过http://127.0.0.1:7070这个地址进行的请求，如果直接将上述代码插入到html文件中，在本地直接访问也会报跨源错误，错误提示中 Origin 由 http://127.0.0.1:7070 变为 `null`。

# JSONP实现跨源通信

JSONP(JSON with Padding) 实现跨源通信的原理是 **Web 页面上调用外部 js 文件不受浏览器同源策略的限制，所以通过动态创建 `script` 便签，然后获取其内容，便实现了跨源通信**，具体过程为：

1. 首先在 DOM 中创建 `script` 标签；
2. 将 `script` 标签的 `src` 属性值设置为请求API的地址（一般是一个绝对路径的url），这里的关键是 **设置回调函数，并将其作为 url 的参数**；
2. 服务端接收到请求后，通过该参数获得回调函数名，并将数据放在参数中将其返回；
3. 获取数据后就可以通过提前设置的回调函数进一步处理数据。

## JSONP实例

还是以调用豆瓣读书API为例，通过JSONP调用的代码如下：

```js
// 创建script标签
const script = document.createElement('script');
script.type = 'text/javascript';

// 指定script标签src属性值，同时传参并指定回调函数，此处为onBack
script.src = 'https://api.douban.com/v2/book/6548683?callback=onBack';
document.head.appendChild(script);

// 回调函数
function onBack(res) {
  console.log(res);
}
```

执行上面代码，就可以跨源访问豆瓣API了。

## 优缺点

- 优点

1. 兼容性很好，在古老的浏览器也能很好的运行；
2. 简单，不管通过创建 `script` 标签获取数据，还是通过回调函数处理获取的数据，都简单明了；
3. 不需要服务端单独为某个域名做设置（对比CORS方法，下文说明），这在请求自己不受控的资源是特别受用（你总不能要求豆瓣或者某个提供查询天气状况的API提供商单独为你的域名做设置）。

- 缺点

1. 只支持 `get`；
2. 潜在的安全问题（大站提供的API可忽略）；
3. 稳定性存疑。毕竟，使用JSONP访问的资源往往都是别人家的，说不定哪天就关了或者代码有改动。跨源

> 如果是商用，不推荐使用JSONP方法，个人项目用的较多。

# CORS实现跨源通信

[CORS](https://fetch.spec.whatwg.org/#http-cors-protocol)(Cross-Origin Resource Sharing，跨源资源共享)是 W3C 一项推荐标准，定义了在必须访问跨源资源时，浏览器和服务器应该如何通信。   

其实现方式是，通过使用自定义的HTTP头信息让浏览器和服务器进行通信，进而确定是否允许跨源通信。   

浏览器向服务器发送请求时会在 Request Headers 中自动添加请求源信息（`Origin`字段），服务器端通过在 Response Headers 中添加 `Access-Control-Allow-Origin` 字段，指明允许那些源可以访问。经浏览器检查，如果请求源在服务器允许请求源列表中，则允许请求源从该服务器上的获取资源。   

> 实际上，浏览器不会完全阻止跨源请求，如果源A要请求源B上的资源，浏览器会将请求到的数据放在 [网络层](https://en.wikipedia.org/wiki/OSI_model#Layer_3:_Network_Layer) ，然后检查源A是否在源B允许的请求源列表上，如果没有，浏览器就会触发 `XMLHttpRequest` 的 `error` 事件（此处就是跨源错误），同时阻止数据从网络层传送到源A的js代码中。

# 参考资料

【1】[How does Access-Control-Allow-Origin header work?](https://stackoverflow.com/questions/10636611/how-does-access-control-allow-origin-header-work)
