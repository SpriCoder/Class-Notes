MYSQL 事务
---

# 1. 事务回滚 ROLLBACK
1. 事务回滚只能使用于DML，不适用于DDL
2. ROLLBACK适用于事务回滚。

## 1.1. 开始事务
`START TRANSACTION` | `BEGIN`

## 1.2. 提交事务
`COMMIT`

## 1.3. 回滚事务
`ROLLBACK`

## 1.4. 使用情景
1. 执行大量的UPDATE和DELETE时，一定要使用事务。

# 2. 参考
1. <a href = "https://blog.csdn.net/u010895119/article/details/81560473">MySQL的rollback--事务回滚</a>