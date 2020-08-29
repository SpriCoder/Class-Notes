Python mysql连接
---

<!-- TOC -->

- [1. MYSQL连接原理](#1-mysql连接原理)
- [2. MySQL连接代码实现](#2-mysql连接代码实现)
- [3. 将csv格式的数据导入mysql](#3-将csv格式的数据导入mysql)
- [4. 参考](#4-参考)

<!-- /TOC -->

# 1. MYSQL连接原理
1. 通过Python3连接MySQL
2. 获取MySQL数据集
3. 利用Python的相应库解析JSON(txt等)格式数据
4. 把解析好数据放到数据集，并放回到MySQL数据库

# 2. MySQL连接代码实现
```py
import pymysql
conn1 = pymysql.connect(host="127.0.0.1",user="root",password="1234",db="mypydb")
x = 'firsttitle'
y = 'firstkeyword'
sql = "INSERT INTO mytb(title,keywd) VALUES('%s','%s')"%(x,y) # sql语句
conn1.query(sql)
conn1.commit()
```

# 3. 将csv格式的数据导入mysql
```sql
load data infile "文件路径.csv"
into table table-name 
/*如果csv文件包含中文，添加"character set gb2313"*/
fields terminated by ',' optionally enclosed by '"' escaped by '"'
lines terminated by '\r\n'
```

# 4. 参考
1. <a href = "https://blog.csdn.net/wcg541/article/details/100749975">如何用Python从数据库里面获取数据</a>
2. <a href = "https://blog.csdn.net/fmyzc/article/details/80435130">如何将csv格式数据导入mysql</a>