bisect模块
---
1. 模块导入:`import bisect`

# 1. 方法
1. `insort(array,number)`:在array中插入number，并且不会破坏原来的排序。
2. `bisect(array,number)`:表示number即将插入到array的位置。但是并不执行插入
3. `bisect_left(array,number)`:表示number即将插入到array的位置，但是优先返回左侧(有重复)
4. `bisect_right(array,number)`:表示number即将插入到array的位置，但是优先返回右侧(有重复)