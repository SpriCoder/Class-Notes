部署到linux服务器
---

<!-- TOC -->

- [1. 将spring boot项目打包](#1-将spring-boot项目打包)
- [2. 上传到服务器](#2-上传到服务器)
- [3. 在服务器端挂起项目](#3-在服务器端挂起项目)
- [4. 参考](#4-参考)

<!-- /TOC -->

# 1. 将spring boot项目打包
1. 以maven为例，首先用`maven clean`处理项目
2. 之后用`maven install`打包为jar

# 2. 上传到服务器
1. 使用xftp进行数据上传。

# 3. 在服务器端挂起项目
1. 使用nohup命令运行:`nohup java -jar filename.jar 2>&1 &`
2. 查看进程:`netstat -ntpl|grep java`
    + 使用命令`netstat -ntpl`查看所有在运行进程。
3. 彻底杀死进程:`kill -9 (pid)`

# 4. 参考
1. <a href = "https://blog.csdn.net/qq_31489805/article/details/80105928">spring boot打包部署</a>
2. <a href = "https://blog.csdn.net/educast/article/details/28273301">spring项目打包过程中的重定向问题</a>