Git 远程分支操作
---
1. 避免污染Master主分支，我们往往直接对于远程分支进行操作。

# 1. 拉取远程分支

## 1.1. 本地创建一个Github仓库
1. `git init`:新建一个仓库

## 1.2. 建立远程连接
1. `git remote add origin git@xxxx`:将本地仓库和远程仓库相关联

## 1.3. 拉取远程分支到本地
1. `git fetch origin branch-name`:拉取远程名字为branch-name的分支到本地

# 2. 提交到github远程分支
1. 使用vscode先从左下角切换分支
2. 推送提交:`git push origin master:remote-branch`

# 3. 参考
1. <a href = "https://www.jianshu.com/p/542109f0e998">github:拉取远程分支到本地</a>
2. <a href = "https://www.jianshu.com/p/94a64b480f9b">git提交到github远程分支</>
   

# 4. 完整操作
```
git init
git remote add origin https://github.com/nju-AU/nju-AU.git
git fetch origin hello
git checkout hello
```