---
date: 2021-01-07
status: public
title: java面试总结
tags:
  - JAVA
---

摘要:java面试总结
<!--more-->
# dubbo底层rpc 原理
ip 和端口 通过socket连接
# mysql主从原理 binlog ->relaylog
# 动态代理跟静态代理啥区别
静态代理是直接代理，就是定义一个接口，然后A类实现它，然后另外B类也实现这个接口，B类里面有A类实现每个方法，这样实现的时候B类就可以代替A类，这就叫静态代理
JDK动态代理是通过invocationhandle实现的 实现一个类实现invocationhandle 实现invoce方法，然后强制转换成对应Proxy类，就不用实现接口的每个方法
CGlib动态代理类是通过MethodInterceptor  实现的，实现一个类实现MethodInterceptor ，实现intercept 方法， 然后强制转换成newProxy类

# cglib动态代理跟jdk动态代理啥区别
接口走jdk动态代理，其他走cjlib
# 怎么实现动态代理，原理
# rebbitmq   exchange 分几类
# 有没看mybatis源码 mybatis怎么映射到dao的
# 线程状态，怎么变成死锁
# synchronized和reentrantlock 底层原理
# synchronized除了cas还有锁升级 
# synchronized 的实现涉及到锁的升级，具体为无锁、偏向锁、自旋锁、向OS申请重量级锁，
# reentrantlock 除了CAS  还有AQS 
# spring 单例模式 工厂模式  观察者模式
# 观察者模式主要用于什么地方
# spring事务是属于哪里的，代码怎么实现
# nio原理
# netty线程模式
# dubbo使用的时候遇到过什么问题
# mysql 建表的时候要注意什么
# mysql中 使用explain时应注意那些字段
# 热部署 冷部署
# docker
# k8s其他功能
# #引用的分类  
## 强引用   jvm 情愿爆出OOM 也不回收的对象
## 弱引用	jvm查到的话就会回收 不管内存够不够 
## 软引用	Jvm 如果内存不够用的时候就会回收
## 虚引用	jvm看到就回收

# 字节面试一面总结
# 算法题 
在一个字符串S内查找一个子串s的出现位置。
public static int indexOf(char[] chars, char[] sub) {
return -1;
}
# 如何保证RabbitMQ不被重复消费？
# 如何保证RabbitMQ消息的可靠传输？
# activeMQ
# dubbo 与springcloud 区别
# zookeeper 单节点还是多节点 如果挂了怎么办
# zk集群是怎么部署的
# 并发锁 分布式锁
# 支付宝微信调用过程中怎么确保安全
# 微信支付宝是怎么做订单的拦截消费
# mysql 索引的实现原理
# mysql 节点存储的是什么 非叶子节点 和叶子节点
# 一个节点最大存储 16K
# 16k是相对于什么来说的
# Username 查询 % 最左前缀原则 为什么

# 浩鲸科技一面
## dubbo重试机制
1，Dubbo支持多种失败重试机制：
Failover Cluster - 失败自动切换
Failfast Cluster - 快速失败
Failsafe Cluster - 失败安全
Failback Cluster - 失败自动恢复
Forking Cluster - 并行调用多个服务提供者
Broadcast - 广播轮询调用所有Provider


## zk服务注册发现过程，如果一个断了，怎么通知到对方
这里写的非常清楚[zk服务注册发现过程](https://blog.csdn.net/zyhlwzy/article/details/101847565)
1，要清楚zk的数据模型，主要是一个树形结构，也就是目录结构 eg:/11/22/33 数据模型中的每一个节点，Zookeeper称之为Znode
2，zk的watcher机制，和znode绑定的一个监听器，当有节点不提供服务的时候自动提示
3，具体流程
1）客户端调用getData方法向服务器获取某个Znode节点的数据时，设置watch为true。
服务端接到请求后，返回节点的数据，并在维护的WatchTable中插入被Watch的Znode路径以及Watcher（watch该Znode的客户端）；
2）当被Watch的Znode被删除或者更新之后，Zookeeper服务器会查找Watch Table，找到在Znode上对应的所有Watcher，
异步通知对应的客户端，并且删除Watch Table中对应的Key：Value；
4，zk的服务注册和发现流程
1）服务注册：服务提供者（Provider）启动时，会向Zookeeper服务端注册服务信息，
即会在Zookeeper服务器上创建一个服务节点，并在节点上存储服务的相关数据（如服务提供者的ip地址、端口等），比如注册一个用户注册服务（user/register）:
2）服务发现：服务消费者（Consumer）启动时，会根据本身依赖的服务信息，向Zookeeper服务端获取注册的服务信息并设置Watch，
获取到注册的服务信息之后将服务提供者信息缓存在本地，调用服务时直接根据从Zookeeper注册中心获取到的服务注册信息调用服务，比如发现用户注册服务（user/register）并调用。
3）服务通知（类似服务心跳检测）：当服务提供者因为某种原因宕机或不提供服务之后，Zookeeper服务注册中心的对应服务节点会被删除，
因为服务消费者在获取服务信息的时候在对应节点上设置了Watch，因此节点删除之后会触发对应的Watcher，
Zookeeper注册中心会异步向服务所关联的所有服务消费者发出节点删除的通知，服务消费者根据收到的通知更新缓存的服务列表。

## zk节点类型
1.持久节点(PERSISTENT)
持久节点，创建后一直存在，直到主动删除此节点。
2.持久顺序节点(PERSISTENT_SEQUENTIAL)
持久顺序节点，创建后一直存在，直到主动删除此节点。在ZK中，每个父节点会为它的第一级子节点维护一份时序，记录每个子节点创建的先后顺序。
3.临时节点(EPHEMERAL)
临时节点在客户端会话失效后节点自动清除。临时节点下面不能创建子节点。
4.顺序临时节点(EPHEMERAL_SEQUENTIAL)
临时节点在客户端会话失效后节点自动清除。临时节点下面不能创建子节点。父节点getChildren会获得顺序的节点列表。

## zk还可以用来做什么，具体怎么实现
1,分布式锁，具体实现方式
1）znode 可以被监控。
节点数据修改、子节点变化、节点删除等。
一旦变化可以通知设置监控的客户端。
通过这个特性可以实现的功能包括配置的集中管理，集群管理，分布式锁等等。
2）分布式锁步骤。
注：zk设置watcher的操作和读操作是原子的。读取子节点列表同时设置监听器
	1. 父节点持久节点/lock
	2. 所有客户端在/lock下创建 瞬时有序节点/lock/seq-0000000000、/lock/seq-0000000001、依次类推
	3. 获取/lock下所有子节点getChildren("/lock")方法，判断自己是否为最小的。是：获取锁； 否：监听自己前一位子节点的删除消息(调用exist()方法，同时注册事件监听。 )，
	获得通知后重复该步骤。
	4. 执行代码。完成后释放锁。
4）使用zookeeper + curator，完成分布式锁。

## redis与mysql 怎么做到缓存一致性
1、MySQL binlog增量发布订阅消费+消息队列+增量数据更新到redis
	1）读请求走Redis：热数据基本都在Redis
	2）写请求走MySQL: 增删改都操作MySQL
	3）更新Redis数据：MySQ的数据操作binlog，来更新到Redis
2、Redis更新
	1）数据操作主要分为两块：
	一个是全量(将全部数据一次写入到redis)
	一个是增量（实时更新）
	这里说的是增量,指的是mysql的update、insert、delate变更数据。
	这样一旦MySQL中产生了新的写入、更新、删除等操作，就可以把binlog相关的消息推送至Redis，Redis再根据binlog中的记录，对Redis进行更新。就无需在从业务线去操作缓存内容
	2）每次写数据库的时候都把redis的数据给删掉，读数据库的时候再保存到redis 缓存
3、延迟双删，一定程度可以解决，就是写数据库之后 睡眠一段时间，然后再删除一次，但是不一定百分百解决问题，而且延迟了主线程
4，分布式锁，可以解决，并发问题串行化，但是效率低
5，读锁，写锁可以一定程度优化，读多写少，读读不互斥，写写互斥

## 怎么保证redis 高可用
redis高并发：主从架构，一主多从，一般来说，很多项目其实就足够了，单主用来写入数据，单机几万QPS，多从用来查询数据，多个从实例可以提供每秒10万的QPS。
redis高并发的同时，还需要容纳大量的数据：一主多从，每个实例都容纳了完整的数据，比如redis主就10G的内存量，其实你最多只能容纳10g的数据量。
如果你的缓存要容纳的数据量很大，达到了几十g，甚至几百g，或者是几t，那你就需要redis集群，而且用redis集群之后，可以提供可能每秒几十万的读写并发。
redis高可用：如果你做主从架构部署，其实就是加上哨兵就可以了，就可以实现，任何一个实例宕机，自动会进行主备切换。
## redis 集群是怎么搭建的
当启动一个 slave node 的时候，它会发送一个 PSYNC 命令给 master node。
如果这是slave node重新连接master node，那么master node仅仅会复制给slave部分缺少的数据;如果这是 slave node 初次连接到 master node，那么会触发一次 full resynchronization 全量复制。
开始full resynchronization的时候，master 会启动一个后台线程，开始生成一份 RDB 快照文件，同时还会将从客户端新收到的所有写命令缓存在内存中。RDB 文件生成完毕后， master 会将这个 RDB 发送给 slave，slave 会先写入本地磁盘，然后再从本地磁盘加载到内存中，接着 master 会将内存中缓存的写命令发送到 slave，slave 也会同步这些数据。
slave node 如果跟 master node 有网络故障，断开了连接，会自动重连，连接之后 master node 仅会复制给 slave 部分缺少的数据。master如果发现有多个slave node都来重新连接，仅仅会启动一个rdb save操作，用一份数据服务所有slave node。
## 分库分表
java中主要是用的是Sharding-JDBC分库分表  整合springboot 非常方便
## 分布式事务是怎么做的seata
1. 二阶段提交协议 (2PC)
	协议中有两类节点：一个中心化协调者 (Coordinator) 节点与 N 个参与者 (Cohort) 节点。
	1），协调者询问所有的参与者是否可以提交事务，所有参与者向协调者投票，同时参与者执行询问发起为止的所有事务操作，并将Undo信息和Redo信息写入本地日志
	2），协调者根据参与者的投票结果，做出是否事务可以全局提交的决定，并通知所有参与者执行该决定，如若一个节点没有回复，那就是通知所有事务回滚。
	缺点 
	1），一旦协调者所在服务器宕机，就会影响整个数据库集群的正常运行
	2），所有的参与者都需要听从协调者的统一调度，期间处于阻塞状态而不能从事其他操作，这样效率极其低下。
	3），两阶段提交协议虽然是分布式数据强一致性所设计，但仍然存在数据不一致性的可能性。比如在第二阶段中，假设协调者发出了事务 commit 通知，
	但是因为网络问题该通知仅被一部分参与者所收到并执行了commit 操作，其余的参与者则因为没有收到通知一直处于阻塞状态，这时候就产生了数据的不一致性。
	
2. 三阶段提交协议 (3PC)
针对两阶段提交存在的问题，三阶段提交协议通过引入一个 预询盘 阶段，以及超时策略来减少整个集群的阻塞时间，提升系统性能。
三阶段提交的三个阶段分别为：预询盘（can_commit）、预提交（pre_commit），以及事务提交（do_commit）。
	1），为甚需要cancommit
	假设有1个协调者，9个参与者。其中有一个参与者不具备执行该事务的能力。
	协调者发出prepare消息之后，其余参与者都将资源锁住，执行事务，写入undo和redo日志。
	协调者收到相应之后，发现有一个参与者不能参与。所以，又出一个roolback消息。其余8个参与者，又对消息进行回滚。这样子，是不是做了很多无用功？
	所以，引入can-Commit阶段，主要是为了在预执行之前，保证所有参与者都具备可执行条件，从而减少资源浪费。
	2)，这种方案解决了服务协调者单点问题，在cancommit 之后 precommit 之后 所有节点在docommit阶段要是超时没有收到协调者信息的时候会默认提交事务
	因为在precommit阶段已经知道所有节点都同意提交事务了
3. 基于消息的分布式事务
	基于rabbitmq的消息队列，每次发送的时候先执行A，然后再发送给rabbitmq 这样就不会消息丢失
## session与token和cookie区别
1，session的中文翻译是“会话”，当用户打开某个web应用时，便与web服务器产生一次session
2，cookie是保存在本地终端的数据。cookie由服务器生成，发送给浏览器，浏览器把cookie以kv形式保存到某个目录下的文本文件内，下一次请求同一网站时会把该cookie发送给服务器
3，token的意思是“令牌”，是用户身份的验证方式，最简单的token组成uid
 cookie 和session的区别
1）、cookie数据存放在客户的浏览器上，session数据放在服务器上。
2）、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗
   考虑到安全应当使用session。
3）、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能
   考虑到减轻服务器性能方面，应当使用COOKIE。
4）、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
5)、所以个人建议：
   将登陆信息等重要信息存放为SESSION
   其他信息如果需要保留，可以放在COOKIE中


# shein一面
## springboot有什么优点 特性
## mysql 插入一千条一万条语句怎么办
INSERT INTO 
items(name,city,price,number,picture) 
VALUES
('耐克运动鞋','广州',500,1000,'003.jpg'),
......
('耐克运动鞋2','广州2',500,1000,'002.jpg');

java代码的话是先用for循环先拼接好，然后再拼接语句insert ，然后再执行代码
## 慢查询怎么看然后怎么优化  举个栗子
## ABC 三列组合索引 这个时候 A= B in C =   这个时候会不会走索引
## 为什么最左前缀只走第一列  最左前缀原理 要清楚存储数据原理 一个节点存储了三个值 代表三个数值
## mybatis 常用标签 foreach
## redis 常用指令 lpush  lset
## rabbitmq 确认机制怎么实现 publisher-confirms
## elk  (Elasticserarch + Logstash + Kibana)
## romda 表达式
分组        Map<String, List<User>> groupBySex = userList.stream().collect(Collectors.groupingBy(User::getSex));
过滤        List<User> userCommonList = userList.stream().filter(a -> !a.getJobNumber().equals("201901")).collect(Collectors.toList());

## 为什么要使用联合索引
### 减少开销。建一个联合索引(col1,col2,col3)，实际相当于建了(col1),(col1,col2),(col1,col2,col3)三个索引。每多一个索引，都会增加写操作的开销和磁盘空间的开销。
对于大量数据的表，使用联合索引会大大的减少开销！
### 覆盖索引。对联合索引(col1,col2,col3)，如果有如下的sql: select col1,col2,col3 from test where col1=1 and col2=2。
那么MySQL可以直接通过遍历索引取得数据，而无需回表，这减少了很多的随机io操作。减少io操作，特别的随机io其实是dba主要的优化策略。
所以，在真正的实际应用中，覆盖索引是主要的提升性能的优化手段之一。
### 效率高。索引列越多，通过索引筛选出的数据越少。有1000W条数据的表，有如下sql:select from table where col1=1 and col2=2 and col3=3,
假设假设每个条件可以筛选出10%的数据，如果只有单值索引，那么通过该索引能筛选出1000W10%=100w条数据，然后再回表从100w条数据中找到符合col2=2 and col3= 3的数据，
然后再排序，再分页；如果是联合索引，通过索引筛选出1000w10% 10% *10%=1w，效率提升可想而知！


# 保利一面
## 提前预警系统问题
## springboot 注解实现原理
## zookeeper有什么缺点  AP 和 CP  效率不高
## dubbo 线程模型
## dubbo怎么做到负载均衡  loadbalance xml配置文件里面直接配置
## 四种负载均衡  
1，随机
2，轮训
3，一致性哈希
4，最少活跃数
## elk 调用链 就是监听requestid log4j 
## 并发高怎么设计系统
1，使用缓存redis
2，线程池
3，nginx 负载均衡
4，消息队列
## redis 过期机制
1，定时删除 在设置某个key 的过期时间同时，我们创建一个定时器，让定时器在该过期时间到来时，立即执行对其进行删除的操作。
2，惰性删除 设置该key 过期时间后，我们不去管它，当需要该key时，我们在检查其是否过期，如果过期，我们就删掉它，反之返回该key。
3，定期删除 每隔一段时间，我们就对一些key进行检查，删除里面过期的key。

# 阿里一面
## 秒杀设计 要考虑什么
1，前端限流 -按钮只能按一次
2，后端限流 -利用MQ削锋
3，后端redis防止超卖
4，后端mysql限制
5，限制IP多次请求，缓存到redis里面
6，
## java代码如何保证原子性 原子加 原子减   AtomicInteger
## synchronized和reentrantlock 区别
## synchronized 可以锁哪些对象，有什么区别
1，对象 变量  对象锁
2，代码块  对象锁
3，类  类锁
1、对于静态方法，由于此时对象还未生成，所以只能采用类锁；
2、只要采用类锁，就会拦截所有线程，只能让一个线程访问。
3、对于对象锁（this），如果是同一个实例，就会按顺序访问，但是如果是不同实例，就可以同时访问。
4、如果对象锁跟访问的对象没有关系，那么就会都同时访问。

## AtomicInteger 原理  CAS volatile修饰的int值 做成原子加 原子减
## 垃圾处理GC 调优 -XMX  -XMS  -XMN


# 美的一面
## SPI  dubbo的spi机制
SPI全称Service Provider Interface，是Java提供的一套用来被第三方实现或者扩展的接口，
它可以用来启用框架扩展和替换组件。 SPI的作用就是为这些被扩展的API寻找服务实现。
@spi 配置它可以扩展
## java spi 
1，一种扩展机制，JDBC实现的接口就是driver，但是这个接口是扩展出去的，很多接口都会实现的 
需要在META-INF的配置文件里面去配置对应service 接口
2，缺点：不能做ioc 和aop 不能获取单独的一个实现类

## ZK集群选举的一个过程
Zab协议

## redis集群 同步原理 rdb aof文件 
## 哨兵怎么跟redis的节点通信的 master  slave节点
哨兵会给所有节点发ping指令，然后其他节点会回复pong指令，如果一段时间都没回复，那就是断开了
## 哨兵选举
1，配置priority 优先级最高的
2，选择复制量最大的
## 主从结构哨兵是否有用，假如一个节点掉了，会到哪里
## mysql集群是用什么来做 模式是怎么样实现的
## mysql主节点和从节点是怎么同步的 binlog 和relaylog
## 如果从库查没有的话，这个时候你要怎么办，你要先保存到redis，之后再存到从节点（主从延迟）
## redis 用于哪些方面  用户信息， 分布式锁， 主从同步缓存数据
## 分布式锁是怎么实现的
redis setnx指令 finally里面delete  命令执行   要先判断一下当前线程ID 是不是自己的，如果是自己的那就可以删除，不然不能删除
## redisson
## mysql优化  开启慢查询 slow_log 开启  explain 一下，然后把对应不走索引的给查出来
## mysql 有哪些索引 原理
普通索引、唯一索引、聚集索引、主键索引、全文索引  B+树

## 索引下推是什么原理
在不使用ICP的情况下，在使用非主键索引（又叫普通索引或者二级索引）进行查询时，存储引擎通过索引检索到数据，
然后返回给MySQL服务器，服务器然后判断数据是否符合条件 。
在使用ICP的情况下，如果存在某些被索引的列的判断条件时，MySQL服务器将这一部分判断条件传递给存储引擎，
然后由存储引擎通过判断索引是否符合MySQL服务器传递的条件，只有当索引符合条件时才会将数据检索出来返回给MySQL服务器 。
索引条件下推优化可以减少存储引擎查询基础表的次数，也可以减少MySQL服务器从存储引擎接收数据的次数。

## mysql 还有其他什么日志吗  undo  redo
## 分布式事务 二段式 三段线   
## 最终一致性 跟分布式事务有什么区别 
## 分布式事务 CAP理论 base理论
## 多线程编程 线程池 类及构造参数
## 线程池是怎么工作的
## 最大线程数 如果队列满了咋办
## worker队列要怎么设置
## 如果队列都满了，这个时候你要放到mq队列
## jvm 内存结构
## jvm 多大就会放到大内存里面 是一个参数
## 老年代full GC  G1 CMS GC 过程

# 美的二面
## dubbo文档
## MQ对比
## 数据库事务写成功之后往MQ 里面写
## 事务中是否需要RPC





## 高可用方案 从前端到后端
前端nginx做负载均衡，redis做查询缓存，mysql集群做负载

1，系统拆分  dubbo 加zk 分布式
2，Cache(缓存)  读多写少 redis做缓存
大部分的高并发场景，都是读多写少，那你完全可以在数据库和缓存里都写一份，然后读的时候大量走缓存不就得了。毕竟人家 redis 轻轻松松单机几万的并发。
3，MQ  把数据库操作先放到MQ 排队执行
大量的写请求灌入 MQ 里，排队慢慢玩儿，后边系统消费后慢慢写，控制在 mysql 承载范围之内
4，数据库拆分(分库分表) Sharding-JDBC
当数据量达到某个阀值时，数据库拆分就会成为一个紧急的需求。一般从业务上进行垂直拆分，如果业务单一，也可从水平上进行拆分。
5，读写分离
主库写入，从库读取，搞一个读写分离。读流量太多的时候，还可以加更多的从库。
6，ElasticSearch
7，CDN 加速  把页面资源CDN加速一下
其中有速度快的路径有慢的路径，如何选择最优路径，把每个角落的请求快速的传递到机房，这就是 CDN 的功能。
8，HTML 页面静态化
静态页面部署在 NGNIX 中，收到用户请求，Ngnix 不需要访问 Webapp 即可响应用户，减少应用渲染页面的时间，同时也降低了应用的压力。

# 卓志
## 微服务的技术选项
易于部署，各个工程各不影响，分模块部署，负载均衡
## dubbo注册与发现过程
服务提供者 容器container 服务调用者 注册中心 counter
## dubbo负载均衡 几种
Dubbo 提供了4种负载均衡实现，分别是基于权重随机算法的 RandomLoadBalance、基于最少活跃调用数算法的 LeastActiveLoadBalance、
基于 hash 一致性的 ConsistentHashLoadBalance，以及基于加权轮询算法的 RoundRobinLoadBalance
## dubbo遇到过什么问题吗？
1，父子类有相同属性时值丢失 
dubbo默认采用的是hessian序列化&反序列化方式，JavaDeserializer在获取fileds时，采用了Map去重。
但是在读取值时，根据serializer的顺序，对于同名字段，子类的该字段值会被赋值两次，总是被父类的值覆盖，导致子类的字段值丢失。
2，序列化问题复杂对象变成了map
3，Data length too large 
超过8k   修改 payload的值，将其调大 可以调节到16k
4，服务调用失败  No provider available
存在一个或多个Provider服务，但是version或者group不匹配。
例如Consumer侧申明version=1.0.0，而Provider侧申明version=2.0.0，或者group不匹配，都会出现这个ERROR。
## dubbo遇到过什么序列化的问题
## dubbo参数对象如果是object属性 可以传递过去吗
最近遇到一个问题，B 服务调用 A 服务时，返回值反序列化时，POJO对象变成了Map类型。
在A服务单独测试的时候一直还原不了，在 B 服务进行测试的时候，跟到反序列化数据时才看到原因。
A 服务的接口方法返回的结果是一个Object（或 Map<String, Object> 中的 value），
Object 的具体实现不在 A 服务的 API 包中，因此在 B 服务找不到该返回值真正的实现类，
在 B 服务调用接口返回结果反序列化找不到具体的类型时，就会以 Map 类型进行实例化。

### Java序列化：
Java序列化会把要序列化的对象类的元数据和业务数据全部序列化为字节流，而且是把整个继承关系上的东西全部序列化了。
它序列化出来的字节流是对那个对象结构到内容的完全描述，包含所有的信息，因此效率较低而且字节流比较大。
但是由于确实是序列化了所有内容，所以可以说什么都可以传输，因此也更可用和可靠。
### hession序列化：
它的实现机制是着重于数据，附带简单的类型信息的方法。就像Integer a = 1，
hessian会序列化成I 1这样的流，I表示int or Integer，1就是数据内容。而对于复杂对象，通过Java的反射机制，
hessian把对象所有的属性当成一个Map来序列化，产生类似M className propertyName1 I 1 propertyName S stringValue
（大概如此，确切的忘了）这样的流，包含了基本的类型描述和数据内容。而在序列化过程中，如果一个对象之前出现过，
hessian会直接插入一个R index这样的块来表示一个引用位置，从而省去再次序列化和反序列化的时间。
这样做的代价就是hessian需要对不同的类型进行不同的处理（因此hessian直接偷懒不支持short），
而且遇到某些特殊对象还要做特殊的处理（比如StackTraceElement）。而且同时因为并没有深入到实现内部去进行序列化，
所以在某些场合会发生一定的不一致，比如通过Collections.synchronizedMap得到的map。
## zk特点 CAP 理论  CP  consist
## zk集群挂了会怎么办会投票 【myid ，zid】
## MQ三大特性 解耦异步削峰
## MQ 消息丢失
## rebbitMQ持久化是怎么持久化
将queue的持久化标识durable设置为true,则代表是一个持久的队列
## redis分布式锁 lua脚本
redisson 
## redis如何保证原子性
单线程的
## 既然是单线程的，如何做到异步和非阻塞的
要明白他的线程模型是IO多路复用模型 主要核心是select channel  bufuer类 就是一个线程负责接收连接，其他线程负责解决对应的事务
## redis 可以用来做什么 
1，缓存  2，分布式锁  3，zset 排名 4，索引 
## redis 秒杀的功能是怎么做的
1，限流和降级 按钮置灰 nginx限流  2，队列削峰MQ  3，服务层分包部署  4，查询的时候查询redis 缓存
## 多线程是怎么用的
多线程newcache 保证线程无线 CPU消耗比较大  newfixed 线程数固定 但是队列无线，内存不友好
## 有无遇到OOM的问题
## jvm垃圾回收机制 标记计数算法 可达性算法 标记清理 标记整理 复制算法
## 几个垃圾收集器  CMS标记清理  G1

## 服务挂了的话怎么办 linux 怎么去查服务器日志
linux日志文件说明
/var/log/message 系统启动后的信息和错误日志，是Red Hat Linux中最常用的日志之一
/var/log/secure 与安全相关的日志信息
/var/log/maillog 与邮件相关的日志信息
/var/log/cron 与定时任务相关的日志信息
/var/log/spooler 与UUCP和news设备相关的日志信息
/var/log/boot.log 守护进程启动和停止相关的日志消息
/var/log/wtmp 该日志文件永久记录每个用户登录、注销及系统的启动、停机的事件

### Java应用程序的问题：发生OOM导致进程Crash
最常见的是发生堆内存异常“java.lang.OutOfMemoryError: Java heap space”，排查步骤如下：
Step1: 查看JVM参数 -XX:+HeapDumpOnOutOfMemoryError 和 -XX:HeapDumpPath=*/java.hprof；
Step2: 根据HeapDumpPath指定的路径查看是否产生dump文件；
Step3: 若存在dump文件，使用Jhat、VisualVM等工具分析即可；
### JVM出错：JVM或JDK自身的Bug导致进程Crash
当JVM出现致命错误时，会生成一个错误文件 hs_err_pid.log，其中包括了导致jvm crash的重要信息，
可以通过分析该文件定位到导致crash的根源，从而改善以保证系统稳定。当出现crash时，该文件默认会生成到工作目录下，
然而可以通过jvm参数-XX:ErrorFile指定生成路径。
### 被操作系统OOM-Killer
Step1: 查看操作系统日志：sudo grep –color “java” /var/log/messages，确定Java进程是否被操作系统Kill；
Step2: 若被操作系统Kill，执行dmesg命令查看系统各进程资源占用情况，明确Java占用内存是否合理，
以及是否有其它进程不合理的占用了大量内存空间；

## 数据库横表与纵表的区分  没什么卵用 都是到横表的 
 横表就是普通的建表方式，如一个表结构为： 主键、字段1、字段2、字段3。。。 如果变成纵表后，
 则表结构为： 主键、字段代码、字段值。 而字段代码则为字段1、字段2、字段3。
## TIDB 最新数据库 
mysql的一个封装，加快查询效率
## tcp的头标记
## rpc 主要框架及对比  dubbo  grpc  thrift dubbox  motan
grpc是一个轻量级的java RPC框架。它支持服务注册和发现。
dubbox有比价完善的服务治理模型，其包含ZK注册中心，服务监控等，可以很方便的为我们服务。 
motan新浪微博开源的RPC框架
grpc是Google出品，使用了PB协议，但是由于它出现的比较晚，还不怎么成熟，而且采用http协议，非常适合现在的微服务，
	不过性能上差了许多，而且像服务治理与监控都需要额外的开发工作，所以放弃grpc。 
thrift和grpc一样，性能优越，但是开发难度相比较于dubbox和motan也是高了一点点，
	需要编写proto文件（其实对于程序员来说这算不上难度）。像服务治理与监控也是需要额外的开发工作。 
dubbo比较老了，直接弃用。 
dubbox和后来的motan都比较适合我们的团队。dubbox后来经过当当的开发，引入了rest风格的http协议，
并且还支持kryo/fst/dubbo/thrift等其他协议，而且其他团队也在使用dubbo，集成方便，服务治理监控功能齐全，所以最终采用dubbox。
其实我个人而言还是喜欢thrift，毕竟性能优越，在大型分布式系统中，哪怕一点点性能提升累计在一起也是很可观的。
不过再具体选型的过程中还要结合团队目前的状况和团队其他开发人员的接受程度进行取舍。


# 易工品
## springcloud alibaba 跟springcloud的区别
注册中心不一样 nacos 集成了Ribbon eureka
## 最新关注技术点 tidb 优点
## 做的业务点
## zk的业务场景 zk的特点
## rocketmq 卡夫卡 和 rabbitmq的比较
吞吐量 ：卡夫卡》rocketmq》rabbitmq
接收的模式：卡夫卡拉取：rabbitmq 推拉
## 所有人都订阅了这个消息，推送消息的话，所有都接收的话卡夫卡合适吗？
## 财务金额用的是什么类型存储，怎么去做取整的操作
## mysql的变更通知
## 单元测试是怎么做的
Junit框架，在大多数java的开发环境中已经集成，
## 生产故障
## 图数据库和时序数据库


## 承担什么责任  做一个项目碰到什么难题  
电商  支付模块 


奥悦家 
用户支付同步天问接口回调很慢的话怎么办  ，然后会导致用户重复缴费两次
1，先异步返回，然后给数据库设置一个在缴费中的标志位，返回给业主说正在缴费中
2，起一个线程去不断查询天问系统接口，然后接收回调接口的返回
3，在缴费之后就先用redis把查询的费用设置为0，就是说先去redis 查询费用，如果没有的话就去天问查，如果有的话就看多少钱
使用dubbo中遇到什么问题
序列化的问题----复杂对象的时候会 变成map
服务调用失败  No provider available-----版本号不一致
Data length too large 超过8k-------修改 payload的值，将其调大 可以调节到16k

奥悦团购  超卖库存的问题 秒杀
1前端
面对高并发的抢购活动，前端常用的三板斧是【扩容】【静态化】【限流】
　　A：扩容
　　加机器，这是最简单的方法，通过增加前端池的整体承载量来抗峰值。
　　B：静态化
　　将活动页面上的所有可以静态的元素全部静态化，并尽量减少动态元素。通过CDN来抗峰值。
　　C：限流
　　一般都会采用IP级别的限流，即针对某一个IP，限制单位时间内发起请求数量。
　　或者活动入口的时候增加游戏或者问题环节进行消峰操作。
　　
2后端
A：将存库从MySQL前移到Redis中，所有的写操作放到内存中，由于Redis中不存在锁故不会出现互相等待，
并且由于Redis的写性能和读性能都远高于MySQL，
这就解决了高并发下的性能问题。然后通过队列等异步手段，将变化的数据异步写入到DB中。
B：引入队列，然后将所有写DB操作在单队列中排队，完全串行处理。
当达到库存阀值的时候就不在消费队列，并关闭购买功能。这就解决了超卖问题。
C：利用Java的锁机制CAS来实现减库存操作
D：写sql乐观锁的概念，增加了版本号，
//1.查询出商品信息
select stock, version from t_goods where id=1;
//2.根据商品信息生成订单
insert into t_orders (id,goods_id) values (null,1);
//3.修改商品库存
update t_goods set stock=stock-1, version = version+1 where id=1, version=version;
当A线程来更新数量的时候先看版本号有没有动，如果已经变了那就不改并且返回，如果没变，这个时候就更改库存，并且把版本号加一
E：分布式锁
setnx指令，如果能拿到对应标志位的锁的话，那就成功获取锁，并且更改库存，如果不行的话，那就直接返回抢购失败



# 中移
## maven调用链依赖重复依赖是怎么解决的
当一个项目中出现重复的依赖包时，maven 2.0.9之后的版本会用如下的规则来决定使用哪一个版本的包：
最短路径原则
比如有如下两个依赖关系：
A -> B -> C -> D(V1)
F -> G -> D(V2)
这个时候项目中就出现了两个版本的D，这时maven会采用最短路径原则，选择V2版本的D，因为V1版本的D是由A包间接依赖的，整个依赖路径长度为3，而V2版本的D是由F包间接依赖的，整个依赖路径长度为2。
声明优先原则
假设有如下两个依赖关系：
A -> B -> D(V1)
F -> G -> D(V2)
这个时候因为两个版本的D的依赖路径都是一样长，最短路径原则就失效了。这个时候Maven的解决方案是：
按照依赖包在pom.xml中声明的先后顺序，优先选择先声明的包
## 