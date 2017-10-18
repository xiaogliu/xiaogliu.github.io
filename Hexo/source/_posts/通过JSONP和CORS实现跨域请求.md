---
title: 通过JSONP和CORS实现跨域请求
date: 2017-10-18 15:06:32
tags: [JavaScript]
categories: JavaScript
e_title: cross-origin-request-using-jsonp-and-cors
---

# 为什么会产生跨域

跨域产生的场景主要是在：如果A页面（通过B页面提供的API）获取B页面的资源，而A页面和B页面的源信息不同（源信息包括：协议、域名、端口三部分），那么便会产生跨域错误。比如调用豆瓣读书的API，
