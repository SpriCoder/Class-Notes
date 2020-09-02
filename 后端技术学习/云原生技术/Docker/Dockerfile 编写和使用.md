Dockerfile 编写和使用
---
1. Dockerfile是一个用来构建镜像的文本文件
2. Dockerfile的指令每一次都会在docker上新建一层，所以应该避免过度无意义的层，防止造成镜像膨胀过大。(使用管道等方式)

# 1. Dockerfile 指令
  
## 1.1. FROM 指令
1. 定制的镜像都是基于FROM的镜像

## 1.2. RUN 指令
1. 用来执行后面跟着的命令行命令
```dockerfile
# shell格式
RUN <命令行命令>

# exec格式
RUN ["可执行文件"，"参数1"，"参数2"]
```

## 1.3. COPY 指令
1. 复制指令，从上下文目录中复制文件或者目录到容器里指定路径
```dockerfile
COPY [--chown=<user>:<group>] <源路径1>...  <目标路径>
COPY [--chown=<user>:<group>] ["<源路径1>",...  "<目标路径>"]
```
2. 源路径可以是源文件或者源目录，也支持通配符表达式
3. 目标路径如果原来没有的话，会自动创建

## 1.4. ADD 指令
1. ADD指令和COPY指令格式一样
   1. 优点:在执行源文件为tar压缩文件的情况下，会自动复制并解压到目标路径。
   2. 缺点:在不解压的前提下，无法复制tar压缩文件，会导致镜像缓存失效。

## 1.5. CMD 指令
1. CMD类似RUN，用于运行程序
2. 但是CMD是在Docker run的时候执行，可以被添加的参数覆盖
3. 如果容器有多个CMD命令，仅执行最后一个

```dockerfile
CMD <shell 命令> 
CMD ["<可执行文件或命令>","<param1>","<param2>",...] 
CMD ["<param1>","<param2>",...]  # 该写法是为 ENTRYPOINT 指令指定的程序提供默认参数
```

## 1.6. ENTRYPOINT 指令
1. 类似CMD命令，但是不会被docker run的命令行参数指定的指令覆盖，并且这些命令会被当做参数被送给ENTRYPOINT指定的程序
2. 但是如果docker run的时候使用了`--entrypoint`选项，则可以进行覆盖。

## 1.7. 区分CMD和ENTRYPOINT指令
1. 假设已经构建好了nginx:test的镜像

```dockerfile
FROM nginx

ENTRYPOINT ["nginx", "-c"] # 定参
CMD ["/etc/nginx/nginx.conf"] # 变参 
```

2. 不传参运行:`docker run nginx:test`，则默认运行:`nginx -c /etc/nginx/nginx.conf`
3. 传参执行:`docker run  nginx:test -c /etc/nginx/new.conf`，则默认运行:`nginx -c /etc/nginx/new.conf`

## 1.8. 区分ENV命令
1. 设置环境变量，如果设置了则之后可以使用
2. 在镜像中也可以使用

```dockerfile
ENV <key> <value>
ENV <key1>=<value1> <key2>=<value2>...
```

2. 例子如下:

```dockerfile
ENV NODE_VERSION 7.2.0

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc"
```

## 1.9. ARG 命令
1. 和ENV命令一样，这个设置的只对Dockerfile有效，而在镜像中不存在
2. 可以在docker build中使用`--build-arg<参数名>=<值>`来覆盖。
```dockerfile
ARG <参数名>[=<默认值>]
```

## 1.10. VOLUME 指令
1. 定义匿名数据卷，在启动容器时忘记挂载数据卷，则会自动挂载到匿名卷
2. 作用
   1. 避免重要的数据，因为容器重启而丢失，这是致命的
   2. 避免容器不断的变大

```dockerfile
VOLUME ["<路径1>", "<路径2>"...]
VOLUME <路径>
```

3. 可以在启动容器时，使用-v参数修改挂载点。

## 1.11. EXPOSE 指令
1. 仅仅是用来声明端口
2. 作用:
   1. 帮助镜像使用者理解这个镜像服务的守护端口，方便进行映射
   2. 在运行时如果使用随机端口映射(`docker run -P`)，则会自动随机映射EXPOSE的端口。

```dockerfile
EXPOSE <端口1> [<端口2>...]
```

## 1.12. WORKDIR 指令
1. 指定工作目录，这个指定的工作目录会在构建镜像的每一层都存在。(这个目录必须是已经提前创建好的)
2. docker build构建镜像过程中的，每一个RUN命令都是新建的一层，只有通过WORKDIR创建的目录才会一直存在。

```dockerfile
WORKDIR <工作目录路径>
```

## 1.13. USER 指令
1. 用于指定执行后续命令的用户和用户组，这边只是切换后续命令执行的用户(用户和用户组必须已经存在)

```dockerfile
USER <用户名>[:<用户组>]
```

## 1.14. HEALTHCHECK 指令
1. 用于指定某个程序或者指令来监控 docker 容器服务的运行状态。

```dockerfile
HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令
HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令

HEALTHCHECK [选项] CMD <命令> : 这边 CMD 后面跟随的命令使用，可以参考 CMD 的用法。
```

## 1.15. ONBUILD
1.  用于延迟构建命令的执行。简单的说，就是 Dockerfile 里用 ONBUILD 指定的命令，在本次构建镜像的过程中不会执行（假设镜像为 test-build）。当有新的 Dockerfile 使用了之前构建的镜像 FROM test-build ，这是执行新镜像的 Dockerfile 构建时候，会执行 test-build 的 Dockerfile 里的 ONBUILD 指定的命令。
2. 格式:
```dockerfile
ONBUILD <其它指令>
```

# 2. Dockerfile构建镜像
1. 构建镜像：`docker build -t name:test .`
2. `.`:表示在当前的上下文路径条件下进行打包