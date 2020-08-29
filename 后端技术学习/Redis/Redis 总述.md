Redis 总述
---

<!-- TOC -->

- [1. 什么是Redis](#1-什么是redis)
- [2. Redis的特点](#2-redis的特点)
- [3. Redis 优势](#3-redis-优势)
- [4. Redis 配置](#4-redis-配置)
- [5. 在线测试Redis](#5-在线测试redis)

<!-- /TOC -->

# 1. 什么是Redis
1. Redis是一个开源的使用ANSI C语言编写、遵守BSD协议、支持网络、可基于内存亦可持久化的日志型、**Key-Value**数据库，并提供多种语言的API。
2. 它通常被称为数据结构服务器，因为值（value）可以是 字符串(String), 哈希(Hash), 列表(list), 集合(sets) 和有序集合(sorted sets)等类型。

# 2. Redis的特点
1. Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。 
2. Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
3. Redis支持数据的备份，即master-slave模式的数据备份。 

# 3. Redis 优势
1. 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
2. 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
3. 原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
4. 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

# 4. Redis 配置
<a href = "https://www.runoob.com/redis/redis-conf.html">Redis 配置</a>

# 5. 在线测试Redis
<a href = "http://try.redis.io/">在线Redis</a>