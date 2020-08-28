<!-- TOC -->

- [1. SQL学习笔记](#1-sql学习笔记)
  - [1.1. 开始](#11-开始)
  - [1.2. 定义](#12-定义)
  - [1.3. 数据库表](#13-数据库表)
  - [1.4. 数据类型](#14-数据类型)
  - [1.5. 连接](#15-连接)
  - [1.6. MySQL 事务](#16-mysql-事务)

<!-- /TOC -->
# 1. SQL学习笔记

## 1.1. 开始

2019年4月12号，这个笔记开始了。  

## 1.2. 定义

>SQL 指结构化查询语言  
>SQL 使我们有能力访问数据库  
>SQL 是一种 ANSI 的标准计算机语言  

**网站中的使用**

>需要元素：
1. RDBMS数据库程序。  
2. 服务端脚本语言。  
3. SQL  
4. HTML/CSS

## 1.3. 数据库表  
一个数据库包含一个或多个表，每个表由一个名字标识，表包带有数据记录（行）。  

## 1.4. 数据类型

**数值类型**

类型|大小|用途
--|--|--
`TINYINT`|1 byte|小整数值
`SMALLINT`|2 byte|大整数值
`MEDIUMINT`|3 byte|大整数值
`INT/INTEGER`|4 byte|大整数值
`BIGINT`|8 byte|极大整数值
`FLOAT`|4 byte|单精度浮点数值
`DOUBLE`|8 bytte|双精度浮点数值
`DECIMAL`|见详见|小数值

**日期和时间类型**

类型|大小(字节)|格式|用途
--|--|--|--
`DATE`|3|YYYY-MM-DD|日期值
`TIME`|3|HH:MM:SS|时间值或持续时间
`YEAR`|1|YYYY|年份值
`DATETIME`|8|YYYY-MM-DD HH:MM:SS|混合日期和时间值
`TIMESTAMP`|4|YYYYMMMDD HHMMSS|混合日期和时间值、时间戳

**字符串类型**

类型|大小|用途
--|--|--
`CHAR`|0-255 b|定长字符串
`VARCHAR`|0-65,535 b|变长字符串
`TINYBLOB`|0-255 b|不超过255个字符的二进制字符串
`TINYTEXT`|0-255 b|短文本字符串
`BLOB`|0-65,535 b|二进制形式的长文本数据
`TEXT`|0-65,535 b|长文本数据
`MEDIUMBLOB`|0-16,777,215 b|二进制形式的中等长度文本数据
`MEDIUMTEXT`|0-16,777,215 b|中等长度文本数据
`LONGBLOB`|0-4,294,967,295 b|二进制形式的极大文本数据
`LONGTEXT`|0-4,294,967,295 b|极大文本数据

**详见**<a href ="http://www.runoob.com/mysql/mysql-data-types.html">查看更详细的部分</a>  



## 1.5. 连接
1. 从多个表中查询数据
2. JOIN按照功能分为三类：
   1. INNER JOIN(内连接或等值连接):获取两个表中字段匹配关系的记录
   2. LEFT JOIN(左连接):获取左表所有记录，即使右表没有对应匹配的记录
   3. RIGHT JOIN(右连接):用于获取右表所有的记录，即使左表没有对应匹配记录
3. 详见<a href="http://www.runoob.com/mysql/mysql-join.html">菜鸟教程</a>

## 1.6. MySQL 事务
1. 用于处理操作量大、复杂度高的数据。比如你在人员管理系统中删除一个人员就要删除和这个人员相关的一切的信息，而这就构成了一个事务。  
>+ 只有使用Innodb数据库引擎的数据库或表才支持事务。
>+ 用于管理insert、update、delete语句。
2. 需要满足的条件：
   + 原子性：要么全完成，要么全部完成。在执行过程中如果错误会回滚到事务开始前。
   + 一致性：在事务开始和结束之后，数据库的完整性没有被破坏。
   + 持久性：数据库允许多个并行事务同时对其收据进行读写和修改的能力，隔离性放置多个事务并发执行时交叉执行导致的数据的不一致。
   >具有不同级别：
      + 读写未提交
      + 读写提交
      + 可重复读
      + 串行化
   + 持久化：事务处理后，对数据的修改都是永久的。
3. 事务控制语句：

事务控制语句|作用
--|--
`BEGIN/START TRANSACTION`|显式地开启一个事务
`COMMIT`|提交事务，并且使得所有对于数据库的修改成为永久的
`ROLLBACK`|回滚结束用户事务，并撤销正在进行的所有未提交的修改
`SAVEPOINT identifier`|允许在事务中创建一个保存点，一个事务中可以有多个SAVEPOINT
`RELEASE SAVEPOINT identifier`|删除一个事务的保存点
`ROLLBACK TO identifier`|把事务回滚到标记点
`SET TRANSACTION`|用来设置事务的隔离级别
4. 事务处理的两种方法：
   1. 用BEGIN,ROLLBACK,COMMIT实现：
      + `BEGIN` 开始一个事务
      + `ROLLBACK` 事务回滚
      + `COMMIT` 事务确认
   2. 直接用SET 改变自动提交模式：
      + `SET AUTOCOMMIT = 1` 开启自动提交
      + `SET AUTOCOMMIT = 0` 禁止自动提交
5. 注意：
   1. 在默认命令行中，事务是自动提交的，也就是执行SQL语句后就会马上commit
   2. <a href = "www.runoob.com/mysql/mysql-transaction.html">实例</a>

