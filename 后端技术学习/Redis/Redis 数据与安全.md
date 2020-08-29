Redis 数据与安全
---

<!-- TOC -->

- [1. 数据备份](#1-数据备份)
- [2. 恢复数据](#2-恢复数据)
- [3. 安全验证](#3-安全验证)

<!-- /TOC -->

# 1. 数据备份
>`redis 127.0.0.1:6379> SAVE`

1. 默认被分到redis安全目录中创建的dump.rdb文件

>`127.0.0.1:6379> BGSAVE`

2. 如上命令我们是在后台执行保存命令

# 2. 恢复数据
1. 将备份文件(dump.rdb)移动到redis安装目录并启动服务即可。获取redis目录也可以使用CONFIG命令

```
redis 127.0.0.1:6379> CONFIG GET dir
1 "dir"
2 "/usr/local/redis/bin"
```

# 3. 安全验证
1. 查看是否设置了密码验证

```
127.0.0.1:6379> CONFIG get requirepass
1 "requirepass"
2 ""
```

2. 设置是否需要密码,以及密码

```
127.0.0.1:6379> CONFIG set requirepass "runoob"
OK
127.0.0.1:6379> CONFIG get requirepass
1 "requirepass"
2 "runoob"
```