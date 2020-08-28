Git 部分报错的解决方案
---

# 1. index文件损坏
> error: bad signature fatal: index file corrupt
1. 解决方案:我们需要的是重新生成index文件

```linux
rm -f .git/index
git reset --mixed HEAD
```