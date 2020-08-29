Python 时间
---
1. 众所周知，time模块和datatime模块。

<!-- TOC -->

- [1. time模块](#1-time模块)
  - [1.1. strftime参数](#11-strftime参数)
- [2. datatime模块](#2-datatime模块)
  - [2.1. 包内模块](#21-包内模块)
  - [2.2. date类](#22-date类)
  - [2.3. time类](#23-time类)
  - [2.4. datetime类](#24-datetime类)
  - [2.5. timedelta类](#25-timedelta类)
- [3. 参考](#3-参考)

<!-- /TOC -->
# 1. time模块
```py
# 导入time模块
import time
# 标准时间戳
time.time()
# 格式化时间戳为本地时间
time.localtime(time.time())
# 优化格式化版本
time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(time.time())))
```

## 1.1. strftime参数
>trftime(format[, tuple]) -> string  
>将指定的struct_time(默认为当前时间)，根据指定的格式化字符串输出
1. python中时间日期格式化符号：

符号|含义
--|--
%y|两位数的年份表示（00-99）
%Y|四位数的年份表示（000-9999）
%m|月份（01-12）
%d|月内中的一天（0-31）
%H|24小时制小时数（0-23）
%I|12小时制小时数（01-12） 
%M|分钟数（00=59）
%S|秒（00-59）
%a|本地简化星期名称
%A|本地完整星期名称
%b|本地简化的月份名称
%B|本地完整的月份名称
%c|本地相应的日期表示和时间表示
%j|年内的一天（001-366）
%p|本地A.M.或P.M.的等价符
%U|一年中的星期数（00-53）星期天为星期的开始
%w|星期（0-6），星期天为星期的开始
%W|一年中的星期数（00-53）星期一为星期的开始
%x|本地相应的日期表示
%X|本地相应的时间表示
%Z|当前时区的名称
%%|%号本身

# 2. datatime模块
```py
# 格式化时间
dt = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
# 转换时间格式
dt_new = datatime.datatime().strptime(dt,'%m/%d/%Y %H:%M').strftime('%Y-%m-%d %H:%M:%S')
```

## 2.1. 包内模块
1. datetime.date：表示日期的类
2. datetime.datetime：表示日期时间的类
3. datetime.time：表示时间的类
4. datetime.timedelta：表示时间间隔，即两个时间点的间隔
5. datetime.tzinfo：时区的相关信息
  
## 2.2. date类
1. date类有三个参数,datetime.date(year,month,day)，返回year-month-day
2. 方法:
   1. datetime.date.ctime():返回当前的时间格式
   2. datetime.date.fromtimestamp(timestamp) 时间戳 => date对象:**这个时间戳不包含小数部分以下的时间戳**
   3. datetime.date.isocalendar():返回格式如(year，month，day)的元组,(2017, 15, 6)
   4. datetime.date.isoformat()：返回格式如YYYY-MM-DD
   5. datetime.date.isoweekday()：返回给定日期的星期（0-6）星期一=0，星期日=6 这里表明下python3中是从[1-7]表示的 就是本来是星期几现在显示就是星期几
   6. datetime.date.replace(year,month,day)：替换给定日期，但不改变原日期
   7. datetime.date.strftime(format):把日期时间按照给定的format进行格式化。
   8. datetime.date.timetuple()：返回日期对应的time.struct_time对象
   9. datetime.date.weekday()：返回日期的星期

## 2.3. time类
1. datetime.time.replace()
2. datetime.time.strftime(format):按照format格式返回时间
3. datetime.time.tzname()：返回时区名字
4. datetime.time.utcoffset()：返回时区的时间偏移量
5. strptime(str,fmt):将字符串时间转换为data对象
  
## 2.4. datetime类
1. datetime.datetime.ctime()   将datetime.datetime类型转化成str类型,输出：Sun Jul 28 15:47:51 2019
2. datetime.datetime.now()：返回当前系统时间：2019-07-28 15:42:24.765625
3. datetime.datetime.now().date()：返回当前日期时间的日期部分：2019-07-28
4. datetime.datetime.now().time()：返回当前日期时间的时间部分：15:42:24.750000
5. datetime.datetime.fromtimestamp():**这个时间戳可以包含小数部分的时间戳**
6. datetime.datetime.strftime()：由日期格式转化为字符串格式
7. datetime.datetime.strptime():由字符串格式转化为日期格式

## 2.5. timedelta类
1. datetime.datetime.timedelta用于计算两个日期之间的差值
```py
a=datetime.datetime.now()
b=datetime.datetime.now()
b-a # datetime.timedelta(0, 8, 732000)
```

# 3. 参考
1. <a href = "https://www.cnblogs.com/komean/p/10603518.html">Python获取当前实践以及格式化</a>
2. <a href = "https://www.cnblogs.com/huigebj/p/11259449.html">Python中datetime库的用法</a>