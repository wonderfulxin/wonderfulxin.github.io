---
date: 2020-03-27
status: public
title: mysql对比小工具
tags:
  - JAVA
  - mysql
---

摘要:mysql对比小工具
<!--more-->
# mysql对比小工具
# 现在测试库跟正式库要做对比，麻烦了 发现了一个小工具很牛逼
下载mysqldiff
下载地址：http://downloads.mysql.com/archives/utilities/

命令模板

     mysqldiff --server1=user:pass@host:port:socket --server2=user:pass@host:port:socket db1.object1:db2.object1 db3:db4

      这里讲的是两种用法。可以直接对比库，db3:db4 ,也可以对比表 db1.table1:db2.table2

      --server1：配置server1的连接。
      --server2：配置server2的连接。
      --character-set：配置连接时用的字符集，如果不显示配置默认使用character_set_client。
      --width：配置显示的宽度。
      --skip-table-options：保持表的选项不变，即对比的差异里面不包括表名、AUTO_INCREMENT、ENGINE、CHARSET等差异。 这个一定要加，否则肯定对比失败。测试环境和正式环境自增字段的当前值肯定不一样。如果是主从对比，就不要加。
      -d DIFFTYPE,--difftype=DIFFTYPE：差异的信息显示的方式，有 [unified|context|differ|sql]，默认是unified。如果使用sql，那么就直接生成差异的SQL，这样非常方便。
      --changes-for=：修改对象。例如 –changes-for=server2，那么对比以sever1为主，生成的差异的修改也是针对server2的对象的修改。
      --show-reverse：在生成的差异修改里面，同时会包含server2和server1的修改。
      --force：完成所有的比较，不会在遇到一个差异之后退出
      -vv：便于调试，输出许多信息
      -q：quiet模式，关闭多余的信息输出


# 例子 
mysqldiff --server1=user:pass@host:port:socket --server2=user:pass@host:port:socket db1.object1:db2.object1 db3:db4

