
---
date: 2021-01-13
status: public
title: springcloud
tags:
  - JAVA
---

摘要:springcloud
<!--more-->
# springcloud 五大组件
## Eureka 服务注册发现有关
一个RESTful服务 ，主要包括两方面一个是Eureka 服务端 一个是Eureka客户端 主要用于服务注册和发现

## Ribbon 负载均衡有关
简单轮询负载均衡
加权响应时间负载均衡
区域感知轮询负载均衡
随机负载均衡

## Hystrix 熔断机制
断路器可以防止一个应用程序多次试图执行一个操作，即很可能失败，允许它继续而不等待故障恢复或者浪费 CPU 周期
断路器增加了稳定性和灵活性，以一个系统，提供稳定性，而系统从故障中恢复，并尽量减少此故障的对性能的影响

## Zuul 网关
类似nginx，反向代理的功能，不过netflix自己增加了一些配合其他组件的特性。

## Spring cloud Config 分布式配置
这个还是静态的，得配合Spring Cloud Bus实现动态的配置更新。

# springcloud 与springcloud alibaba 什么区别
## nacos 注册中心  
## 