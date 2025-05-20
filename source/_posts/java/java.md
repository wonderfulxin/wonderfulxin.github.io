---
date: 2020-04-1
status: public
title: java面试
tags:
  - JAVA
---

摘要:java
<!--more-->

# BIO NIO AIO 
BIO 同步阻塞式IO NIO 同步非阻塞IO  AIO 异步非阻塞IO
BIO 就是一个连接就是一个线程，每来一个连接就起一个新线程，线程开销很大，这样子就导致服务器变慢或者崩溃
NIO 就是全部都用selector()接口连接，全部都用一个线程来接收，其中netty zk 都是用的NIO ，这个时候会有一个线程去轮询当前连接的状态，
AIO 就是一个线程，但是全部连接都到channel，如果有状态可以了，自动就会通知对应的线程来处理
IO多路复用，Mio 

# 单例模式 内存可见性 互斥性  重排序 volatile   synchronized

# IOC 和 AOP DI
依赖注入和切面编程
所谓控制反转是指，本来被调用者的实例是由调用者来创建的，这样的缺点是<font color=red>耦合性</font>太强，
IOC则是统一交给spring来管理创建，将对象交给容器管理，
你只需要在spring配置文件总配置相应的bean，以及设置相关的属性，让spring容器来生成类的实例对象以及管理对象。
在spring容器启动的时候，spring会把你在配置文件中配置的bean都初始化好，然后在你需要调用的时候，
就把它已经初始化好的那些bean分配给你需要调用这些bean的类。

DI 就是依赖注入，当你需要哪个bean的时候，你就通过注解注入某个对象所需要的外部资源（包括对象、资源、常量数据）

AOP对OOP的一个补充，例如你管理后台要登录，你要验证身份token，你不可能每个接口都写一个验证，这个时候你可以在切面写一个拦截，就可以了

# 对BeanFactory ApplicationContext的理解
1，Spring实现了工厂模式的工厂类，这个类名为BeanFactory(接口)，
在程序中通常用他的子类ApplicationContext。
Spring相当于一个大的工厂类，在其配置文件中通过<bean>元素配置用于
创建实例对象的类名和实例对象的属性
2，ApplicationContext包含BeanFactory的所有功能，ApplicationContext还有加载配置文件，触发类路径扫描等功能

# springboot 自动配置 @springbootapplication
这里包括三个配置，@SpringBootConfiguration @EnableAutoConfiguration @ComponentScan
springbootconfiguration enableautoconfiguration compomemtscan

# spring bean 生命周期
1Spring启动，查找并加载需要被Spring管理的bean，进行Bean的实例化
2Bean实例化后对将Bean的引入和值注入到Bean的属性中
3如果Bean实现了BeanNameAware接口的话，Spring将Bean的Id传递给setBeanName()方法
4如果Bean实现了BeanFactoryAware接口的话，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入
5如果Bean实现了ApplicationContextAware接口的话，Spring将调用Bean的setApplicationContext()方法，将bean所在应用上下文引用传入进来。
6如果Bean实现了BeanPostProcessor接口，Spring就将调用他们的postProcessBeforeInitialization()方法。
7如果Bean 实现了InitializingBean接口，Spring将调用他们的afterPropertiesSet()方法。类似的，如果bean使用init-method声明了初始化方法，该方法也会被调用
8如果Bean 实现了BeanPostProcessor接口，Spring就将调用他们的postProcessAfterInitialization()方法。
9此时，Bean已经准备就绪，可以被应用程序使用了。他们将一直驻留在应用上下文中，直到应用上下文被销毁。
10如果bean实现了DisposableBean接口，Spring将调用它的destory()接口方法，同样，如果bean使用了destory-method 声明销毁方法，该方法也会被调用。

# spring bean 作用域
1，singleton 单例模式，每个容器中只有一个bean
2，prototype 为每一个bean 请求创建一个实例
3，request 为每一个request请求创建一个实例，在请求完成之后会失效然后被垃圾回收
4，session 与request 范围类似  同一个session回话共享一个实例，不听会话使用不通过实例
5，global-session 全局作用域，所有会话共享一个实例