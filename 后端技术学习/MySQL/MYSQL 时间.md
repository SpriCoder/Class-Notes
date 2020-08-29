MYSQL 时间
---

# 1. 将日期转化为字符串
1. `select date_format(time, '%Y-%m-%d %H:%i:%s') from  info   # 2020-08-22 21:03:21`
2. <a href = "https://www.cnblogs.com/jichi/p/11396272.html">MYSQL将日期转为字符串</a>

# 2. 如何将datatime格式的数据转换为时间戳
`UNIX_TIMESTAMP(param)`