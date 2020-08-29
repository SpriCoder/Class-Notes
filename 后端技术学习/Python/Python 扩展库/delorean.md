delorean
---

<!-- TOC -->

- [1. 什么是Delorean库](#1-什么是delorean库)
- [2. 具体的使用](#2-具体的使用)
  - [2.1. 安装Delorean](#21-安装delorean)
  - [2.2. 导入](#22-导入)
  - [2.3. 使用](#23-使用)
  - [2.4. 转为国内时区](#24-转为国内时区)
  - [2.5. 可以打印datatime、date](#25-可以打印datatimedate)

<!-- /TOC -->

# 1. 什么是Delorean库
1. 是Python里一个很方便的日期时间库，可以让人更加简单省心的获取时间，而不必困于模块中的各种问题。

# 2. 具体的使用

## 2.1. 安装Delorean
`pip install Delorean`

## 2.2. 导入
`from delorean import Delorean`

## 2.3. 使用
```py
d = Delorean()
print(d)
Delorean(datetime = datetime(2018, 5, 10, 8, 52, 23, 560811), timezone='UTC')
# 这里默认的是UTC时间
```

## 2.4. 转为国内时区
```py
d = d.shift("Asia/Shanghai")
print(d)
Delorean(datetime=datetime(2018, 5, 10, 16, 52, 23, 560718), timezone='Asia/Shanghai')
```

## 2.5. 可以打印datatime、date
```py
print(d.datetime, d.date)
# 2018-05-10 16:58:22.397155+08:00 2018-05-10
# datetime 完整时间 date 日期
```