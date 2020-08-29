Redis 连接
---

<!-- TOC -->

- [1. Redis客户端的基本语法](#1-redis客户端的基本语法)
- [2. 测试客户端的连通情况](#2-测试客户端的连通情况)
- [3. 连接远程的redis服务](#3-连接远程的redis服务)
- [4. Redis 服务器](#4-redis-服务器)
- [5. 管道技术](#5-管道技术)
- [6. 分区技术](#6-分区技术)

<!-- /TOC -->

# 1. Redis客户端的基本语法
`$redis-cli`

# 2. 测试客户端的连通情况
```
redis 127.0.0.1:6379> PING
PONG
redis 127.0.0.1:6379> AUTH "password"
OK
```
<a href = "https://www.runoob.com/redis/redis-connection.html">参考</a>

# 3. 连接远程的redis服务
`$ redis-cli -h host -p port -a password`

# 4. Redis 服务器
<a href = "https://www.runoob.com/redis/redis-server.html">Redis 服务器</a>

1. 最大连接数:
```
config get maxclients
1 "maxclients"
2 "10000"
```

2. 启动时设置最大连接数

```
redis-server --maxclients 100000
```
# 5. 管道技术
<a href = "https://www.runoob.com/redis/redis-pipelining.html">管道测试</a>

# 6. 分区技术
<a href = "https://www.runoob.com/redis/redis-partitioning.html">Redis 分区</a>