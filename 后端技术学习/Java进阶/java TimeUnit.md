Java TimeUnit
---
>所属包:`java.utils.concurrent .TimeUnit `

# 1. TimeUnit 和 阻塞线程
1. TimeUnit帮助指定睡眠时间长度
2. 支持指定`DAYS`、`HOURS`、`MINUTES`、`SECONDS`、`MILLISECONDS`和`NANOSECONDS`
3. 是Java中使用枚举场景非常出色的例子

```java
TimeUnit.MINUTES.sleep(4);  // sleeping for 4 minutes
TimeUnit.SECONDS.sleep(5) //线程休眠5秒 
```

# 2. TimeUnit 和 时间转换
```java
//关于秒的常用方法 
TimeUnit.SECONDS.toMillis(1) //1秒转换为毫秒数 
TimeUnit.SECONDS.toMinutes(60) //60秒转换为分钟数 
TimeUnit.SECONDS.convert(1, TimeUnit.MINUTES) //1分钟转换为秒数 

//TimeUnit.DAYS 日的工具类 
//TimeUnit.HOURS 时的工具类 
//TimeUnit.MINUTES 分的工具类 
//TimeUnit.SECONDS 秒的工具类 
//TimeUnit.MILLISECONDS 毫秒的工具类
```