<!-- TOC -->

- [1. SELECT 语句](#1-select-语句)
- [2. SELECT DISTINCT 语句](#2-select-distinct-语句)
- [3. WHERE 字句](#3-where-字句)
- [4. ORDER BY 子句](#4-order-by-子句)
- [5. GROUP BY 语句](#5-group-by-语句)
- [6. 聚合函数](#6-聚合函数)
- [7. INSERT Into 语句](#7-insert-into-语句)
- [8. UPDATE 语句](#8-update-语句)
- [9. DELETE 语句](#9-delete-语句)
- [10. LIKE 子句](#10-like-子句)
- [11. UNION 操作符](#11-union-操作符)
- [12. ALTER 命令](#12-alter-命令)
  - [12.1. 参考](#121-参考)
- [13. JOIN](#13-join)
  - [13.1. LEFT JOIN](#131-left-join)
  - [13.2. RIGHT JOIN](#132-right-join)
- [14. LIMIT](#14-limit)
- [15. TOP](#15-top)
- [16. SQL函数](#16-sql函数)
- [17. explain](#17-explain)
  - [17.1. 解释](#171-解释)
  - [17.2. Extra列返回的描述的含义](#172-extra列返回的描述的含义)
- [18. AUTO_INCREMENT](#18-auto_increment)

<!-- /TOC -->

DML语句(数据操作语言)|作用
--|--
`SELECT`|从数据库表中获取数据
`UPDATE`|更新数据库表中的数据 ENGINE = InnoDB(标准引擎)
`DELETE`|从数据库表中删除数据
`INSERT INTO`|向数据库表中插入数据

DDL语句(数据定义语言)|作用
--|--
`CREATE DATABASE`|创建新的数据库
`ALTER DATABASE`|修改数据库
`CREATE TABLE`|创建新表
`ALTER TABLE`|更改数据库表
`DROP TABLE`|删除表
`CREATE INDEX`|创建索引
`DROP INDEX`|删除索引

# 1. SELECT 语句
1. 从表中选取数据，结果存在一个表中。  
2. ***语法***
>`SELECT column_name FROM table_name[WHERE claues][LIMIT N][OFFEST M]`  
>>列名称可以是多个并列列，*是通配符，所有列的含义。
>>可以使用一个表或者多个表，用逗号分隔
>>WHERE 来包含任何条件
>>LIMIT 用来设定返回的记录数
>>可以使用OFFSET指定SELECT语句开始查询的数据偏移量。默认偏移量为0

>`SELECT FROM` 表名称  

# 2. SELECT DISTINCT 语句
1. 取出表中不同的值，去除重复的值。  

# 3. WHERE 字句  
1. 用于SELECT中筛选某些条件。  
2. ***语法***  
>`SELECT * FROM table_name WHERE condition1 [AND(OR)] condition2 `  

>**条件部分**  

>操作符|描述
>-----|----
>`=`|等于
>`<>`|不等于
>`>`|大于
>`<`|小于
>`>=`|大于等于
>`<=`|小于等于
>`BETWEEN`|在某个范围内
>`LIKE`|搜索某种模式

>在条件部分，如果是文本值使用**单引号**,如果是数值不加。  

3. **AND & OR 运算符**   
   + `AND`用于两个条件的合取
   + `OR`用于两个条件的或关系
   + 由此产生复杂逻辑表达式

# 4. ORDER BY 子句
1. 用于对结果集进行排序   
2. **语法**  
`SELECT field1, field2,...fieldN table_name1, table_name2...`  
`ORDER BY field1, [field2...] [ASC [DESC]]`
>+ 默认按照升序进行排列,关键字`ASC`。
>+ 如果希望降序排列，使用`DESC`关键字(加在最后)
>+ `OrderNumber`关键字，按照数字顺序显示顺序号  

# 5. GROUP BY 语句
1. 根据一个列或多个列对结果集进行分组。可以使用COUNT,SUM,AVG等函数。
2. **语法**
`SELECT column_name, function(column_name)`  
`FROM table_name`  
`WHERE column_name operator value`  
`GROUP BY column_name;`  
3. WITH ROLLUP:在分组统计数据基础上在进行相同的统计  
4. 在使用GROUP BY 字句的时候：
   1. 生成了一个虚拟表，所以个单元格中会有多个值，这也就是为什么不能用`*`
   2. 那么如何解决在生成的虚拟表中有的单元格是有这个多个值的操作呢？聚合函数
5. <a herf = "https://www.runoob.com/mysql/mysql-group-by-statement.html">参考</a>

# 6. 聚合函数
1. sum(number),cout(id)这一类的聚合函数就解决的多个值的一定程度的解决

# 7. INSERT Into 语句
1. 用于向表格插入新的行。    
2. **语法**
+ `INSERT INTO table_name(field1,field2,...fieldN) VALUES (值1，值2，......)`
+ 如果数据是字符型的必须使用单引号或者双引号  
+ 插入新的行
+ `INSERT INTO table_name (列1，列2，......) VALUES (值1，值2，......)`
+ 向指定列中插入数据

# 8. UPDATE 语句
1. 用于修改表中的数据  
2. **语法**
>`UPDATE table_name SET field1 = new-value1,field2 = new-value2 [WHERE clause]`  
>其中列名称=新值可以多个同时赋值  

# 9. DELETE 语句
1. 用于删除表中的行  
2. **语法**  
>`DELETE FROM table_name [WHERE Clause]`  
3. 删除所有行(但是没有删除表):`DELETE * FROM table_name`  

# 10. LIKE 子句
1.  用于部分筛选
2. **语法**
>`SELECT field1, field2,...fieldN FROM table_name WHERE field1 LIKE condition1 [AND [OR]filed2 = 'somevalue'`
3. 可以在WHERE字句中使用LIKE
4. `%`用于表示任意字符，如果没有则相当于`=`

# 11. UNION 操作符
1. 用于连接两个以上的SELECT语句的结果组合到一个结果集合中。多个SELECT语句会删除重复的数据。
2. **格式**
>`SELECT expression1, expression2, ...  expression_n`  
 `FROM tables`  
    `[WHERE conditions]`  
    `UNION [ALL | DISTINCT]`  
    `SELECT expression1, expression2, ... expression_n`  
    `FROM tables`  
    `[WHERE conditions]`  
   + "expression"：要检索的列
   + "tables"：要检索的数据表
   + "WHERE condition":可选，检索条件
   + "DISTINCT"：可选，删除重复数据(没有意义)
   + "ALL"：可选，返回所有的结果集，包含重复数据
3. 但是在覆盖的过程中，如果主键相同，则前表的数据无条件覆盖后表的数据，会导致一些问题
4. 所有从经验上来讲，我们应该在最后再进行集合函数的使用。

# 12. ALTER 命令
1. 用于修改数据表名或者修改数据表字段。
2. **语法**  
```
完整语法格式
{ ADD COLUMN <列名> <类型>
| CHANGE COLUMN <旧列名> <新列名> <新列类型>
| ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
| MODIFY COLUMN <列名> <类型>
| DROP COLUMN <列名>
| RENAME TO <新表名> }
```

命令|作用
--|--
`ALTER TABLE testalter_tbl  DROP i;`|用于删除表上的i字段
`mysql> ALTER TABLE testalter_tbl ADD i INT;`|用于添加列，添加i字段(指定位置插入：`FIRST`设定位第一列`AFTER`用于设定某个字段之后)
3. **子句**  

语法|作用
--|--
`ALTER TABLE testalter_tbl MODIFY c CHAR(10);`|用于修改字段的类型
`ALTER TABLE testalter_tbl CHANGE i j BIGINT;`|用于修改字段的类型，跟着的是要修改的字段，之后是修改后的字段名和类型
`ALTER TABLE testalter_tbl RENAME TO alter_tbl;`|用于修改数据表的名称

## 12.1. 参考
<a href = "http://c.biancheng.net/view/2433.html">MySQL修改数据表（ALTER TABLE语句）</a>

# 13. JOIN

## 13.1. LEFT JOIN
1. 该关键字会从左表返回所有的行，即便右表中没有对应的边。
2. 语法
```sql
SELECT column_name(s)
FROM table_name1
LEFT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```
3. <a href = "https://www.w3school.com.cn/sql/sql_join_left.asp">参考</a>

## 13.2. RIGHT JOIN
1. 该关键字会从右表返回所有的行，即便左表中没有对应的边
2. 语法
```sql
SELECT column_name(s)
FROM table_name1
RIGHT JOIN table_name2 
ON table_name1.column_name=table_name2.column_name
```

# 14. LIMIT

# 15. TOP

# 16. SQL函数
1. max()
2. min()
3. count()

# 17. explain
1. explain显示了mysql如何使用索引来处理select语句以及连接表。
2. 有效地帮助我们选择更好的索引和写出更优化的查询语句
3. 使用语法`explain sql_sentense`

## 17.1. 解释
1. table:解释数据是哪一张表的
2. type:解释来那连接使用了何种类型。
    1. type显示的是访问类型，是较为重要的一个指标，结果值从好到坏依次是：system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL
    2. 一般来说，得保证查询**至少达到range级别**，最好能达到ref。
3. possible_keys:显示可能应用于这张表的索引
4. key:表示实际使用的索引，可以使用SELECT语句中使用USE INDEX (indexname) 来强制使用一个索引或者IGNORE
5. key_len:使用索引的长度，不损失精度的情况下，长度越短越好
6. ref:显示索引的哪一列别使用
7. rows:认为必须检查的用来返回请求数据的行数。
8. Extra:关于MYSQL如何解析查询的额外信息。

## 17.2. Extra列返回的描述的含义
1. <a href = "https://www.cnblogs.com/yycc/p/7338894.html">详见</a>

# 18. AUTO_INCREMENT
1. 如果没有指定的话，默认从1号开始进行填充。
2. 如果使用 AUTO_INCREMENT = n 字段可以指定从n开始进行填充。