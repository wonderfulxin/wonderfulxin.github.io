---
date: 2020-11-26
status: public
title: aop
tags:
  - JAVA
---

摘要:aop
<!--more-->
# aop 实现
AspectJ 注解实现  @Around @after @before

# aop 是怎么获取参数的
aopAspect 里面可以通过Autowired 获取到 HttpServletRequest  然后可以获取到参数 和对应的ip 等等

# 过滤器filter、拦截器interceptor、和AOP的区别与联系
## Filter过滤器
过滤器拦截web访问url地址。 严格意义上讲，filter只是适用于web中，依赖于Servlet容器，利用Java的回调机制进行实现。
Filter过滤器：和框架无关，可以控制最初的http请求，但是更细一点的类和方法控制不了。
过滤器可以拦截到方法的请求和响应(ServletRequest request, ServletResponse response)，并对请求响应做出像响应的过滤操作，
比如设置字符编码，鉴权操作等

## Interceptor拦截器
拦截器拦截以 .action结尾的url，拦截Action的访问。 Interfactor是基于Java的反射机制（APO思想）进行实现，不依赖Servlet容器。
拦截器可以在方法执行之前(preHandle)和方法执行之后(afterCompletion)进行操作，回调操作(postHandle)，可以获取执行的方法的名称，
请求(HttpServletRequest)
Interceptor：可以控制请求的控制器和方法，但控制不了请求方法里的参数(只能获取参数的名称，不能获取到参数的值)
（用于处理页面提交的请求响应并进行处理，例如做国际化，做主题更换，过滤等）。

## Spring AOP拦截器
只能拦截Spring管理Bean的访问（业务层Service）。 具体AOP详情参照 Spring AOP：原理、 通知、连接点、切点、切面、表达式
实际开发中，AOP常和事务结合：Spring的事务管理:声明式事务管理(切面)
AOP操作可以对操作进行横向的拦截，最大的优势在于他可以获取执行方法的参数( ProceedingJoinPoint.getArgs() )，对方法进行统一的处理。
Aspect : 可以自定义切入的点，有方法的参数，但是拿不到http请求，可以通过其他方式如RequestContextHolder获得(
ServletRequestAttributes servletRequestAttributes= (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
)。
常见使用日志，事务，请求参数安全验证等

##简单总结一下，拦截器相比过滤器有更细粒度的控制，依赖于Spring容器，可以在请求之前或之后启动，过滤器主要依赖于servlet，过滤器能做的，拦截器基本上都能做。

## Filter与Interceptor联系与区别
拦截器是基于java的反射机制，使用代理模式，而过滤器是基于函数回调。
拦截器不依赖servlet容器，过滤器依赖于servlet容器。
拦截器只能对action起作用，而过滤器可以对几乎所有的请求起作用（可以保护资源）。
拦截器可以访问action上下文，堆栈里面的对象，而过滤器不可以。
执行顺序：过滤前-拦截前-Action处理-拦截后-过滤后。


## AOP 原理 （动态代理）
### JDK动态代理
### Cglib 动态代理

## 动态代理怎么选择
### 接口走Cglib，其他走JDK 还有个参数isProxyTarget可以设置 EnableAspectJAutoProxy

## 什么是动态代理
动态生成类，不再需要代理
## 动态代理和静态代理区别
静态代理是你写一个接口 然后 一个类继承这个接口，然后要代理这个类 你要额外写一个类然后继承这个接口，然后去代理你原来这个类，需要实现所有接口方法
动态代理你写一个接口，然后 一个类继承这个接口，利用反射机制在运行时创建代理类，我们构建一个handler类来实现InvocationHandler接口，实现invoke方法
然后调用newProxyInstance 强制转换成这个接口，就可以代理这个类了