---
date: 2020-03-27
status: public
title: servlet原理
tags:
  - JAVA
  - servlet
---

摘要:servlet原理
<!--more-->
# servlet 生命周期
首先加载servlet的class，实例化servlet，然后初始化servlet调用init()的方法，
接着调用服务的service的方法处理doGet和doPost方法，最后是我的还有容器关闭时候调用destroy 销毁方法。
![avatar](/img/servlet1.jpeg)

从这张图上面可以看到整个生命周期，请求到来-通过tomcat找到对应web-通过配置文件找到servlet
-找到并且实例化-调用init-调用service方法-找到对应类-处理问题实现doget dopost-destroy销毁


