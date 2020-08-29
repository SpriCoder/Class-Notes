SQL的部分错误
---

<!-- TOC -->

- [1. 缺省值问题](#1-缺省值问题)
- [2. 语法问题](#2-语法问题)
- [3. 插入语句失败问题](#3-插入语句失败问题)
- [4. 语句中存放emoji表情失败的问题](#4-语句中存放emoji表情失败的问题)

<!-- /TOC -->

# 1. 缺省值问题
>ERROR 1101 (42000): BLOB/TEXT column can’t have a default value
1. 问题背景:写SQL语句，创建库的时候设置DEFAULT中有不当值。
2. <a href = "https://blog.csdn.net/A11085013/article/details/21695617">mysql 报错 ERROR 1101</a>

# 2. 语法问题
>Caused by: java.sql.SQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'interval, is_loop, remarks, standard_id' at line 1
1. 问题背景:在数据库后，使用spring框架注入时出现问题
2. 问题解决:因为数据库列的名字和SQL中保留字发生冲突
    + 比如:like

# 3. 插入语句失败问题
>在我们使用mybatis框架下的插入语句的时候，我们发现无法进行插入，此时我们进行查找资料发现，condition作为列名是我们的MYSQL的保留字。
1. 解决方案:我们调整数据库列的名字不为SQL的保留字。
2. 比如:`order`、`rank`

# 4. 语句中存放emoji表情失败的问题
1. 将对应字段的排序规则修改为:`utf8mb4_general_ci`
2. 将连接数据库的charset修改成:`utf8mb4`