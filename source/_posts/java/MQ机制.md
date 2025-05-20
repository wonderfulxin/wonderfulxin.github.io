---
date: 2020-03-17
status: public
title: MQ机制
tags:
  - JAVA
  - MQ
---

摘要:MQ机制
<!--more-->
# MQ 消息对接的作用
## 解耦

## 削峰

## 异步

# rebbitMQ
RabbitMQ是使用Erlang编写的一个开源的消息队列，吞吐量和tps 适中，性能较好，erlang语言较难维护

# activeMQ（开源给了apollo）又叫ApolloMQ
维护较少，会丢失数据

# kafka
吞吐量，和tps 都大，适合做大数据

# 三者区别
## TPS 每秒传输的事务处理个数
Kafka最高，RabbitMq 次之， ActiveMq 最差
## 吞吐量对比 处理事务的能力
kafka具有高的吞吐量，内部采用消息的批量处理，rabbitMQ在吞吐量方面稍逊于kafka，rabbitMQ支持对消息的可靠的传递，支持事务，不支持批量的操作
ActiveMq 最差

# rebbitMQ Exchange分几类
1，topic  就是direct的升级版 就是增加了一个路由的通配符
2，direct 绑定队列和路由 只有设定了特定路由的队列才可以收到
3，fanout  广播模式 任何队列都可以收到
4，headers

# 如何保证RabbitMQ不被重复消费？（幂等性）
1.当拿到这个消息做数据库的insert操作。那就容易了，给这个消息做一个唯一主键，那么就算出现重复消费的情况，就会导致主键冲突，避免数据库出现脏数据。
2.当拿到这个消息做redis的set的操作，那就容易了，不用解决，因为你无论set几次结果都是一样的，set操作本来就算幂等操作。
3.只要消费过该消息，将<id,message>以K-V形式写入redis。那消费者开始消费前，先去redis中查询有没消费记录即可
4.如果消费的时候，两三个线程一起来消费，要注意这个时候要采用CAS 乐观锁的一个概念，就是说给数据库表加多一个字段，如果消费了就改变它状态，这个时候其他消费就不能消费了

# 如何保证RabbitMQ消息的可靠传输？
1，生产者报错，生产者要保证消息的可靠性传输有两个办法
1），直接用事务的概念，报错直接rollback
2），启用confirm模式，发送的时候会发送一个ACK 标志，一旦消息投递到队列里面就会发送 ACK，如果rabbitmq没有处理这个消息就会发送NACK 给你，这个时候你可以重试
2，rabbitmq 本身报错，这个时候可以开启持久化机制，只要写入进来就写到硬盘里面，这个时候如果报错的话，下次重启还是可以继续读出来
3，消费者报错，这个时候可以把ACK机制手动确认，只有消费者消费之后才返回ACK，这样就可以在消费报错的时候，让生产者重传

# rabbitmq 怎么实现延迟消息队列？
使用 RabbitMQ-delayed-message-exchange 插件实现延迟功能。

# rabbitMQ 事务是怎么实现的
通过AMQP事务机制实现，这也是AMQP协议层面提供的解决方案；
txSelect(), txCommit()以及txRollback(), txSelect用于将当前channel设置成transaction模式，
txCommit用于提交事务，txRollback用于回滚事务，在通过txSelect开启事务之后，我们便可以发布消息给broker代理服务器了，
如果txCommit提交成功了，则消息一定到达了broker了，如果在txCommit执行之前broker异常崩溃或者由于其他原因抛出异常，
这个时候我们便可以捕获异常通过txRollback回滚事务了。
