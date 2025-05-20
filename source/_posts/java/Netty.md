
---
date: 2021-01-12
status: public
title: Netty
tags:
  - JAVA
---

摘要:Netty底层原理
<!--more-->
# Netty 线程模型  对NIO的封装
Reactor模型 ---》NIO模型   默认是一个主从多路复用模型
不过也要看这个EventLoopGroup设置
NioEventLoopGroup默认线程数为CPU核心数的两倍
如果new了两个NioEventLoopGroup，且指定工作线程数不为1，则是主从多线程模型
如果new了两个NioEventLoopGroup，且指定工作线程数为1，则是主从单线程模型
如果new了一个NioEventLoopGroup，且指定工作线程数不为1，则是多线程模型
如果new了一个NioEventLoopGroup，且指定工作线程数为1，则是单线程模型

# Reactor模型
事件监听  selector  多路复用    先注册到selector 然后来一个事件就监听起来    用一个selector去接受线程  另外一个是去执行io

# Nio Bio Aio
nio 单线程然后通过selector 获取进入队列，然后通过一个线程去循环查询当前是否释放资源，就可以去执行   channel 非阻塞的
ServerSocketChannel  SocketChannel  buffer Selector多路复用选择器里面注册  nio 是一个线程selector 去做连接，连接好了的socket 就注册进另外一个selector

## NIO 组成
selector channel（ServerSocketChannel SocketChannel） buffer

## EventLoop 与EventLoopGroup什么关系
NioEventLoopGroup 是NioEventLoop的组合，用于管理NioEventLoop

bio 来一个服务就搞一个socket  然后把这些线程都存在arraylist里面    来一个服务就起一个线程，对CPU伤害极大
aio 异步


