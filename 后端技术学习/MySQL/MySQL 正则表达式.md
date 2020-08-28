<!-- TOC -->

- [1. 正则表达式](#1-正则表达式)
- [2. 实例](#2-实例)
- [3. 参考](#3-参考)

<!-- /TOC -->

# 1. 正则表达式
1. 同样的，我们也可以在sql中匹配的时候使用正则表达式。
2. 正则表达式语法见参考外链。
3. 关键字:`REGEXP`

# 2. 实例
```sql
# 查找name中以st为开头的所有数据
SELECT name FROM person_tbl WHERE name REGEXP '^st';
# 查找name中以ok为结尾的所有数据
SELECT name FROM person_tbl WHERE name REGEXP 'ok$';
# 查找name字段中包含mar字符段的所有数据
SELECT name FROM person_tbl WHERE name REGEXP 'mar';
# 查找name中以元音字符开头或以ok字符串结尾的所有数据
SELECT name FROM person_tbl WHERE name REGEXP '^[aeiou]|ok$';
```

# 3. 参考
<a href = "http://www.runoob.com/mysql/mysql-regexp.html">正则表达式</a>