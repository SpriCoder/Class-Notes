Redis 安装
---
<!-- TOC -->

- [1. 本地Redis的安装](#1-本地redis的安装)
  - [1.1. 本地Redis系统的Windows版本安装](#11-本地redis系统的windows版本安装)
  - [1.2. 本地启动Redis](#12-本地启动redis)
- [2. 本地Redis系统的Linux版本安装](#2-本地redis系统的linux版本安装)
  - [2.1. 参考服务器安装策略](#21-参考服务器安装策略)
  - [2.2. 如何注册成为serivce](#22-如何注册成为serivce)
  - [2.3. 启动redis服务器](#23-启动redis服务器)
  - [2.4. 部分报错的解决方案](#24-部分报错的解决方案)
  - [2.5. 参考](#25-参考)
- [3. Docker安装Redis](#3-docker安装redis)
  - [3.1. 修改Redis的密码](#31-修改redis的密码)
  - [3.2. 通过替换redis.conf来配置](#32-通过替换redisconf来配置)

<!-- /TOC -->

# 1. 本地Redis的安装

## 1.1. 本地Redis系统的Windows版本安装
1. redis的中文官网:`http://www.redis.cn/`
   + redis的官方团队并没有windows版本的，而是linux版本的
2. 下载windows版本的redis:`https://github.com/MicrosoftArchive/redis`
   1. 下载我们点击releases，然后找到Assets中的对应版本进行下载
   2. 将压缩包解压到指定的盘
   3. 然后打开文件夹，找到redis.windows.conf文件，使用编辑器打开，找到requirepass foobared，复制到对应行，删除前面的#，并且清楚空格，不然会导致密码设置失败，并且将foobared改为自己需要的密码，之后进行保存
3. 打开redis所在的文件夹的地址输入cmd回车进入命令窗口
4. 输入`redis-server.exe redis.windows.conf`命令回车，打开redis服务
5. 不关闭上面一个窗口，重新打开之后输入`redis-cli.exe -h 127.0.0.1 -p 6379 -a password`回车(password是之前设置的密码)
6. 之后使用set命令对key进行存储，使用get key获取存储的值
7. <a href = "https://blog.csdn.net/salestina/article/details/83114598">参考</a>

## 1.2. 本地启动Redis
1. 在对应的文件中cmd进入命令行
2. 首先启动本地redis的服务端:`redis-server.exe redis.windows.conf`
3. 之后我们登入redis服务部分:`redis-cli.exe -h 127.0.0.1 -p 6379 -a password`
4. 如果不显示DB的图片可能是:
   1. 需要修改 `protected-mode  yes` 改为：`protected-mode no`
   2. 需要调整spring配置中redis:timeout时间不为0

# 2. 本地Redis系统的Linux版本安装
1. 在Linux系统上安装
2. 在官网中 `http://www.redis.cn/` 的下载窗口
3. 具体下载过程见官网

## 2.1. 参考服务器安装策略
1. 官网下载:`https://redis.io/download`
```
# 下载
$ wget http://download.redis.io/releases/redis-5.0.8.tar.gz
$ tar xzf redis-5.0.8.tar.gz
$ cd redis-5.0.8
$ make

# 启动服务 ./redis-server和./redis-cli都是在src命令下
$ cd src
$ ./redis-server

# 使用配置文件启动
$ cd src
$ ./redis-server ../redis.conf

# 启动客户端
$ cd src
$ ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

## 2.2. 如何注册成为serivce
1. 进入utils中
2. 执行:`./install_server.sh`

## 2.3. 启动redis服务器
```
service redis start
service redis stop
service redis restart
```

## 2.4. 部分报错的解决方案
1. <a href = "https://blog.csdn.net/weixin_44110998/article/details/103464714">报错(/etc/init.d/redisd: line 28: /usr/local/bin/redis-server: No such file or directory)的解决办法</a>
2. <a href = "https://www.pianshen.com/article/4961770370/">Linux下把redis配置成系统服务报Please select the redis executable path [] Mmmmm... it seems like you错误</a>

## 2.5. 参考
1. <a href = "https://www.cnblogs.com/wmqiang/p/10570326.html">Linux服务器安装部署redis</a>

# 3. Docker安装Redis
1. 搜索镜像:`docker search redis`
2. 拉取镜像:`docker pull redis`
3. 创建docker容器并设置密码:`docker run --name redis -p 6379:6379 redis-test --requirepass 123456`

## 3.1. 修改Redis的密码
1. 查看redis服务的容器ID:`docker ps -a`，找到容器ID
2. 进入redis容器:`docker exec -it 容器ID bash`
3. 进入redis配置目录:`cd /usr/local/bin`
4. 运行命令:`redis-cli`
5. 查看现有的redis密码:`config get requirepass`
6. 设置redis密码:`config set requirepass new_password`

## 3.2. 通过替换redis.conf来配置
1. 在编写dockerfile的时候替换配置文件

```sh
docker run -p 6379:6379 -v /usr/local/redis/conf/redis.conf:/etc/redis/redis.conf -v /usr/local/redis/data:/data --name byteblogs-redis -d redis:5.0 redis-server /etc/redis/redis.conf
```

2. 解释
```
-p 6379:6379 : 将容器的6379端口映射到主机的6379端口
-v /usr/local/redis/conf/redis.conf:/etc/redis/redis.conf :将主机/usr/local/redis/conf目录下redis.conf挂在到容器/etc/redis/redis.conf
-v /usr/local/redis/data:/data  : 将主机 /usr/local/redis/data目录下的data挂载到容器的/data
```