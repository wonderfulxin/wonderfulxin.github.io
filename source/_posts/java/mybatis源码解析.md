---
date: 2021-01-09
status: public
title: mybatis源码解析
tags:
  - JAVA
  - mysql
  - mybatis
---

摘要:mybatis源码解析
<!--more-->
#写在前面-------------------------上次面试的时候问到mybatis源码，自感没看过，有点虚，就去看了源码
# mybatis源码解析
mybatis工作原理
1，XmlConfigBuilder加载property文件获取url password等配置 到Configuration
2，XmlMapperBuilder加载xml文件获取mapper
3，返回SqlSessionFactory.opensession  获取 SqlSession 调用Executor执行数据库操作
4，连接sql，执行mapper，最底层也是通过sql jdbc  SqlSession包含了执行sql所需要的所有方法


# mybatis一级二级缓存

1，一级缓存的粒度较小，是与某个SqlSession绑定的，只对该SqlSession的相关查询操作进行缓存，
不同SqlSession实例之间相互不影响，缓存为使用本地内存实现；mybatis的一级缓存是默认开启的。LocalCache
默认开启不用配置 ；xml setting 配置为localCacheScope 为STATEMENT 则关闭一级缓存

2，二级缓存是一种全局缓存，是由所有SqlSession实例所共享的，即不同SqlSession实例查询时产生的缓存，对其他SqlSession实例可见。
mybatis默认没有开启二级缓存，二级缓存支持在配置中自定义底层所用的缓存实现，包括使用本地内存和分布式缓存。

3，二级缓存是基于namespace的，即作用域为mapper，故需要在每个mapper中配置自身所使用的二级缓存实现以及缓存策略。
同时由于二级缓存是基于namespace的，所以不同namespace之间的相互不影响的，如一个namespace使用的本地内存，
另外一个namespace使用的是分布式缓存，如果不同namespace对同一张数据表的数据进行了操作，则可能会存在数据不一致问题。

## SESSION级别
1,对该SqlSession实例发起的查询操作进行缓存，即由同一SqlSession实例发起的多次相同（SQL和SQL的参数值都相同）的查询操作，
第一次是查询数据库，后续则查询缓存；但是如果另外一个SqlSession实例进行相同的查询操作，则需要进行数据库查询。
2,针对更新操作，如果是该SqlSession自身进行了更新操作，则该SqlSession对应的一级缓存会被清空；
但是如果是其他SqlSession实例进行了更新操作，则此更新操作对该SqlSession不可见。
所以该SqlSession的缓存数据是过期失效数据，所以SqlSession实例的生命周期不能过长，否则可能出现数据不一致现象。
## STATEMENT级别
该级别是指缓存只针对当前执行的查询语句有效，故每次语句执行完之后都会清空缓存，其实是相当于没有缓存，
即该sqlSession实例下次调用相同的SQL语句和相同参数值时，由于上一次语句执行后，缓存被清空了，故需要继续查询数据库

## 怎么才能每次都去数据库查询
配置xml文件flushcacherequired

## 什么时候会清理缓存
1，SqlSession内执行更新操作，包括insert/update/delete，都通过PerpetualCache的clear方法清空localCache
2，执行commit/rollback时也会清空缓存
3，配置中设置localCacheScope为STATEMENT，执行完查询后再清空缓存
4，配置xml设置flushcacherequired，每次查询都清空LocalCache

##  LruCache实现原理
Lru算法，就是超量时移除最长时间不被使用的对象。LruCache使用有序集合LinkedHashMap来实现。
在setSize方法时，将内部的keyMap创建为LinkedHashMap，同时重写了removeEldestEntry方法，
判断如果超过size，则返回最早的元素。

## mybatis ORM框架，用于java 与mysql直接的一个转换，之前java要连接sql的话是写JDBC，现在是mybatis直接转换映射 

## mybatis 加载mapper 有几种方式  
四种 resource url class package  package优先级最高

##  mybatis 有多少种执行器
三种 simple reuse batch  默认 simple