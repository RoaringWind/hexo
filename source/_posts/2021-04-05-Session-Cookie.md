---
title: Session & Cookie
date: 2021-04-05 14:15:27
tags: [计算机网络]
---

Session：用来存放用户数据，浏览器**第一次**发送请求的时候，服务器生成了Session和Session ID来唯一标识这个Session。**第二次**发送请求的时候带上Session ID，就能对应上。

Session的实现方法：

1. 使用Cookie。如果不设置过期时间，就不放在硬盘上，浏览器关闭就消失。如果设置为几天后过期，那么就会在硬盘上保存。
2. 使用URL附加信息。比如```?JSESSIONID=```不设置过期时间，浏览器关闭就过期。

Session放在服务端，Cookie放在客户端。