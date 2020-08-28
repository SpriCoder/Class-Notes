<!-- TOC -->

- [1. 临时表](#1-临时表)
- [2. 复制表](#2-复制表)
  - [2.1. 元数据](#21-元数据)
  - [2.2. 序列](#22-序列)
  - [2.3. 重复数据](#23-重复数据)
  - [2.4. 导出数据](#24-导出数据)
  - [2.5. 导入数据](#25-导入数据)
  - [2.6. explain 查询搜索计划](#26-explain-查询搜索计划)

<!-- /TOC -->
# 1. 临时表
1. 用于保存一些临时数据时是非常有用的，临时表只在当前链接可见。
2. 删除：`DROP TABLE`

# 2. 复制表
1. 用于完整的复制MYSQL的数据表，包含标的结构，索引，默认值等。
2. 基本步骤：
   1. 获取创建数据表语句：`SHOW CREATE TABLE`：包含原数据表的结构，索引等。
   2. 复制一下命令现实的SQL语句，修改数据表表名，并执行SQL语句，通过以上命令将

## 2.1. 元数据

命令|描述
--|--
`SELECT VERSION()`|服务器版本信息
`SELECT DATABASE()`|当前数据库名（或者返回空）
`SELECT USER()`|当前用户名
`SHOW STATUS`|服务器状态
`SHOW VARIABLES`|服务器配置变量

## 2.2. 序列
1. 一张数据表只能有一个字段自增主键，如果你想实现别的字段的自动增加，就可以使用序列
2. 使用方法  
`AUTO_INCREMENT `用于定义列的序列的开始，可以在创建表的时候写在引擎后面也可以在创建表后进行修改。  

## 2.3. 重复数据
1. 防止出现重复数据:  
把某些字段声明为`PRIMARY KEY（主键）`或者`UNIQUE（唯一）`来保证某一个字段的唯一性。
>双主键需要保证那个键值不为null
2. 统计重复数据的步骤
   + 确定哪一行的数据有可能重复
   + 在列选择列表使用COUNT(*)列出的那些列
   + 在GROUP BY子句中列出的列 
   + HAVING子句设置重复数大于1 
实例：  
```SQL
SELECT COUNT(*) as repetitions, last_name, first_name
FROM person_tbl
GROUP BY last_name, first_name
HAVING repetitions > 1;
```
3. 过滤重复数据
   1. 如果你需要读取不重复的数据可以在 SELECT 语句中使用 DISTINCT 关键字来过滤重复数据。  
   实例：  
```SQL
SELECT DISTINCT last_name, first_name
FROM person_tbl;
```
   2. 使用GROUP BY
   实例：  
```SQL
SELECT last_name, first_name
FROM person_tbl
GROUP BY (last_name, first_name);
```
4. 删除重复数据
   1. 实例：  
```SQL
CREATE TABLE tmp SELECT last_name, first_name, sex FROM person_tbl  GROUP BY (last_name, first_name, sex);
DROP TABLE person_tbl;
ALTER TABLE tmp RENAME TO person_tbl;
```
   2. 使用索引和主键：  
```SQL
ALTER IGNORE TABLE person_tbl
ADD PRIMARY KEY (last_name, first_name);
```

## 2.4. 导出数据
`SELECT...INTO OUTFILE "filename"`：  
   1. 用于导出数据，其中filename为写入的文件，要求FILE权限。
   2. 输出不能是一个已存在的文件，防止文件数据被篡改。

## 2.5. 导入数据
`LOAD DATA INFILE filename INTO TABLE tablename`

<a href = "http://www.runoob.com/mysql/mysql-functions.html">SQL函数</a>  
<a href = "http://www.runoob.com/mysql/mysql-operator.html">SQL运算符</a>

## 2.6. explain 查询搜索计划

**注意**  
1. SQL对于大小写不敏感  
2. null值的处理：
>1. IS NULL:如果行值为null，返回true
>2. IS NOT NULL:如果行值不为null,返回true
>3. <=>:如果比较的两个值为null是返回true
>4. 可以设定null初值，如果不设置的话就是null
