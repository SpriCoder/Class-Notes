MySQL
---
1. 通过编写SQL语句实现一页只显示10条记录
```sql
Select * from 
  (Select * from comment where cid = #{cid} and tid = -1 order by createTime DESC) as c
    limit #{page},10
# 这里是指从page开始的10条记录
```