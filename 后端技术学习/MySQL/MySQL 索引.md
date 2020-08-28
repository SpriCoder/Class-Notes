<!-- TOC -->

- [1. 索引](#1-索引)
  - [1.1. 分类：](#11-分类)
    - [1.1.1. 普通索引](#111-普通索引)
    - [1.1.2. 唯一索引](#112-唯一索引)
  - [1.2. 联合索引](#12-联合索引)
- [2. 索引操作](#2-索引操作)
  - [2.1. 查看索引](#21-查看索引)
  - [2.2. 调整索引结构](#22-调整索引结构)
  - [2.3. 删除索引](#23-删除索引)

<!-- /TOC -->

# 1. 索引

1. 索引可以大大提高MySQL的检索速度，但是会占用磁盘中的索引文件

## 1.1. 分类：
1. 单列索引
2. 组合索引

### 1.1.1. 普通索引  
语法|作用
----|---
`CREATE INDEX indexName ON mytable(username(length)); `|创建索引方式一，如果是CHAR，VARCHAR类型，length可以小于字段实际长度；如果是BLOB和TEXT类型，必须指定 length。
`ALTER table tableName ADD INDEX indexName(columnName)`|添加索引
代码块1|添加索引，在创建表的时候直接指定索引。
`DROP INDEX [indexName] ON mytable; `|
删除索引
**代码块1**
```SQL
CREATE TABLE mytable( 
ID INT NOT NULL, 
username VARCHAR(16) NOT NULL,  
INDEX [indexName] (username(length))
);
```

### 1.1.2. 唯一索引
1. 特点：列的值唯一，但是允许有空值。如果是组合索引，则列值的组合必须唯一。
2. 语法  
语法|作用
--|--
`CREATE UNIQUE INDEX indexName ON mytable(username(length)) `|创建索引
`ALTER table mytable ADD UNIQUE [indexName] (username(length))`|修改表结构
代码块2|创建表的时候直接指定索引
**代码块2**
```SQL
CREATE TABLE mytable(  
ID INT NOT NULL,
username VARCHAR(16) NOT NULL,
UNIQUE [indexName] (username(length))
);
```

## 1.2. 联合索引
1. 使用联合索引来提高索引速度
2. 标准格式：`EXPLAIN SELECT* FROM sth WHERE sth AND sth...`
3. 理解：
   1. 利用索引中的附加列，我们可以所想搜索的范围，但是用一个具有两列的索引，不同于使用两个单独的索引。
   2. 复合索引的结构与电话簿相似，因此在创建复合索引时，应该仔细考虑列的顺序，对索引中的所有列执行搜索或仅对前几列执行搜索时，复合索引非常有用，仅对后面的任意列执行搜索时，复合索引则没有用处。
   3. 注意：多个单列索引在多条件查询时只会生效第一个索引，所以联合查询时，我们最好建立联合索引
   4. 最左前缀原则：
      1. 以最左边的为起点的任何连续的索引都可以被匹配上
      2. 一般在建立联合索引的时候，我们将where子句中使用的最频繁的一列放在最左边，增强扩展性
   5. 联合索引的本质：如果建立（a,b,c）的联合索引的，相当于建立了（a）的单列索引、（a,b）的联合索引、（a,b,c）的联合索引。
   6. 如果WHERE中条件时OR的情况下，加索引没有作用。
   7. 联合索引比对每个列分别建立索引更有优势，索引越多，那么占据的磁盘空间就越大，更新数据就会变慢，应将严格的索引放在前面，这样子筛选的力度会更大，效率更高。

# 2. 索引操作

## 2.1. 查看索引
`show index from mytable;//mytable是表名`

## 2.2. 调整索引结构
1. 使用ALTER命令添加和删除索引  
有四种方式添加索引：
   + `ALTER TABLE tbl_name ADD PRIMARY KEY (column_list)`:该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL。
   + `ALTER TABLE tbl_name ADD UNIQUE index_name (column_list)`:这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）。
   + `ALTER TABLE tbl_name ADD INDEX index_name (column_list)`:添加普通索引，索引值可出现多次。
   + `ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list)`:该语句指定了索引为 FULLTEXT ，用于全文索引。
2. 注意：
   1. 在创建索引是要确定索引是应用于WHERE语句的条件，索引也是一个表，类似于指针

## 2.3. 删除索引
1. 用来删除唯一索引:`alter table table_name drop index index_name;`