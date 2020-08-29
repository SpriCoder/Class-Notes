Redis 扩展功能
---

<!-- TOC -->

- [1. Redis HyperLogLog](#1-redis-hyperloglog)
- [2. 发布订阅](#2-发布订阅)
- [3. 事务](#3-事务)
- [4. 脚本](#4-脚本)
- [5. 性能测试](#5-性能测试)

<!-- /TOC -->

# 1. Redis HyperLogLog
1. Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。
2. 比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。 
3. <a href = "https://www.runoob.com/redis/redis-hyperloglog.html">更多参考</a>

# 2. 发布订阅
1. Redis发布订阅(pub/sub)是一种消息通信模式:发送者(pub)发送信息，订阅者(sub)接收信息。
2. Redis客户端可以订阅任何数量的客户
3. 满足了分布式的需求
4. <a href = "https://www.runoob.com/redis/redis-pub-sub.html">参考</a>

# 3. 事务
1. Redis事务可以可以依次执行多个命令，并且带有以下三个重要的保证。
   1. 批量操作在发送 EXEC 命令前被放入队列缓存。 
   2. 收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
   3. 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。
2. 事务从开始到执行会经历以下阶段:
   1. 开始事务:`MULTI`
   2. 命令入队:`具体命令`:会提示:`QUEUED`
   3. 执行事务:`EXEC`

# 4. 脚本
1. Redis 脚本使用 Lua 解释器来执行脚本。 Redis 2.6 版本通过内嵌支持 Lua 环境。执行脚本的常用命令为`EVAL`
2. 基本语法:`redis 127.0.0.1:6379> EVAL script numkeys key [key ...] arg [arg ...]`
3. 例子:

```
redis 127.0.0.1:6379> EVAL "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}" 2 key1 key2 first second

1 "key1"
2 "key2"
3 "first"
4 "second"
```

# 5. 性能测试
1. `redis-benchmark [option] [option value]`
2. 测试命令:`$ redis-benchmark -n 10000  -q`

<a href = "https://www.runoob.com/redis/redis-benchmarks.html">具体测试命令</a>