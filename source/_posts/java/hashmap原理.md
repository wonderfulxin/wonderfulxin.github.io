---
date: 2020-03-27
status: public
title: hashmap原理
tags:
  - JAVA
  - hashmap
---

摘要:hashmap原理
<!--more-->
# hashmap原理
首先1.7之前它是数组加链表的方式来实现的，这个数组就是哈希桶，默认哈希桶的长度是16（2*4次方）

# put方法
把数据的key先哈希然后获取到对应值，然后对数组长度进行取模，然后获取到下标，然后把数据的key，value，next，head，都存到entry里面作为一个节点
当在一个哈希桶里面有值的时候，就对比key是否相同，这里是要通过equal方法进行比较的，相同的话就替换，不相同的话就存到下一个节点，然后把节点entry保存形成链表。

# 扩容
有个扩容因子是0.75，扩容因子可以形成一个阀值，当这个值达到之后，就会扩容，就是表示放不下了，扩容的话先把哈希桶加倍16->32，然后再把所有的原有哈希
节点再rehash一遍到新的哈希桶里面，阀值计算公式就是哈希桶的长度乘以扩容因子，当size就是整个hashmap的数据长度，如果大于等于这个阀值的话就扩容

# get方法
比较简单，就是通过获取到key值然后哈希之后获取到的值对数组长度进行取模，然后获取到对应的下标，然后逐一对比equal，如果有相同就取出

# hashmap的for循环方式
1,iterator
2,for (Map.Entry<String, Integer> entry : testMap.entrySet()) 
3,使用foreach方式（JDK1.8才有）

# jdk1.8之后不同点
jdk1.7链表不断增加，查询效率也在不断减慢，这个时候优化查询是关键
引入了红黑树
entry 也变成了 node
数据插入跟jdk1.7有区别，1.7是头插法，1.8是尾插法

### 步骤①：若哈希table为null，或长度为0，则做一次扩容操作；
### 步骤②：根据index找到目标bucket后，若当前bucket上没有结点，那么直接新增一个结点，赋值给该bucket；
### 步骤③：若当前bucket上有链表，且头结点就匹配，那么直接做替换即可；
### 步骤④：若当前bucket上的是树结构，则转为红黑树的插入操作；
### 步骤⑤：若步骤①、②、③、④都不成立，则对链表做遍历操作。
    a) 若链表中有结点匹配，则做value替换；
    b）若没有结点匹配，则在链表末尾追加。同时，执行以下操作：
       i) 若链表长度大于TREEIFY_THRESHOLD，则执行红黑树转换操作；
       ii) 若条件i) 不成立，则执行扩容resize()操作。
以上5步都执行完后，再看当前Map中存储的k-v对的数量是否超出了threshold，若超出，还需再次扩容。

# hashmap   hashtable  ConcurrentHashMap
hashmap线程不安全，hashtable用了synchronized关键字，线程安全，但是效率不高
在JDK7版本及以前，ConcurrentHashMap类使用了分段锁的技术（segment + Lock），segment 又用了ReentrantLock
但在jdk8中，也做了较大改动，使用回了synchronized修饰符+cas技术

# hashset原理
hashset原理，底层是hashmap实现的，但是把所有的value放到key 里面存放，这样就会保证key 不重复，然后value的值都是private static final Object PRESENT = new Object();

