---
title: arthas使用
date: 2025-05-16 15:06:47
tags:
  - JAVA
  - arthas
---
摘要:arthas使用小技巧

# arthas的使用去排查线上问题

## 安装
直接解压arthas包，[arthas下载包](https://arthas.aliyun.com/doc/download.html)，
点击之后进入阿里的下载页面
### 用 as.sh 启动
```./as.sh ```
### 用 arthas-boot 启动
```java -jar arthas-boot.jar```

#线上问题排查
## 查看线上代码跟本地代码是否一致
通过jad命令编译远程代码可以查看当前代码是否跟本地一致
```jad org.hibernate.EmptyInterceptor```

## 通过watch命令可以查看方法的出参，入参

```watch com.midea.ext.common.util.WFCommonUtil getQueueStatus  '{params,returnObj}'  -x 3 -m 140```

## 打印当前的dump日志
dump java heap, 类似 jmap 命令的 heap dump 功能。

```heapdump arthas-output/dump.hprof```
生成文件在arthas-output目录


