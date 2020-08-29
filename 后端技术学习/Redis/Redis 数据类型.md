Redis 数据类型
---
1. Redis支持五种数据类型:string、hash、list、set、zset
2. 操作返回为1表示成功，0则表示失败

<!-- TOC -->

- [1. String(字符串)](#1-string字符串)
- [2. Hash(哈希)](#2-hash哈希)
- [3. List(列表)](#3-list列表)
- [4. Set(集合)](#4-set集合)
- [5. zset(sorted set:有序集合)](#5-zsetsorted-set有序集合)

<!-- /TOC -->

# 1. String(字符串)
1. key-value格式
2. 二进制安全(可以存储jpg图片或者序列化的对象)
3. 最大存储长度 512MB
4. 每个hash可以存储2<sup>32</sup>-1键值对

<a href = "https://www.runoob.com/redis/redis-strings.html">更多命令</a>

```
SET runnob "123"
GET runnob
```

# 2. Hash(哈希)
1. Redis hash是一个键值(key-value)集合
2. 是一个string类型的field和value的映射表，hash特别适合存储对象
3. 要先删除之前存储的键才能进行基础存储

<a href = "https://www.runoob.com/redis/redis-hashes.html">更多命令</a>

```
HMSET runoob field1 "Hello" field2 "World"
HGET runoob field1
HGET runoob field2
```

# 3. List(列表)
1. Redis类表是简单的字符串列表，按照插入顺序排序。
2. 插入元素到列表左部:`lpush runnob redis`
3. 插入元素到列表右边:`rpush runnob mogo`
4. 开始查看元素:`lrange runnob 0 10`

<a href = "https://www.runoob.com/redis/redis-lists.html">更多命令</a>

# 4. Set(集合)
1. Set是string类型的无序集合
2. 集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)
3. 如果为0表示已经在其中了
4. 插入元素:`sadd runoob redis`
5. 查看元素:`smembers runoob`
6. 集合可以存储2<sup>32</sup>-1键值对

<a href = "https://www.runoob.com/redis/redis-sets.html">更多命令</a>

# 5. zset(sorted set:有序集合)
1. zset是不允许重复的，成员是唯一的但是score是可以重复的
2. 每一个元素都关联一个double的分数
3. Redis通过score完成集合中的排序
4. 添加命令:`zadd key score member`
5. 查看命令:`ZRANGEBYSCORE runnob 0 1000`
   1. 按照分数从小到大排序

<a href = "https://www.runoob.com/redis/redis-sorted-sets.html">更多命令</a>