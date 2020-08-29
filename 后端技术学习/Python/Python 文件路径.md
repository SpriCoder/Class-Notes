python文件路径
---

<!-- TOC -->

- [1. 文件路径](#1-文件路径)
  - [1.1. 绝对路径](#11-绝对路径)
  - [1.2. 相对路径](#12-相对路径)
    - [1.2.1. 参考](#121-参考)

<!-- /TOC -->

# 1. 文件路径
1. 既然需要处理文件，那我们不得不说，文件的相对路径和绝对路径是我们必须要解决的问题。

## 1.1. 绝对路径
1. 绝对路径很好理解，也就是在我们当前的文件系统下的绝对路径。
2. 但是存在相应的问题，在我们移植到服务器上的时候显然我们会出现找不到相应文件的问题。

## 1.2. 相对路径
1. “/”：表示根目录，在windows系统下表示某个盘的根目录，如“E:\”；

2. “./”：表示当前目录；（表示当前目录时，也可以去掉“./”，直接写文件名或者下级目录）
3. “../”：表示上级目录。

![](https://s2.ax1x.com/2020/01/01/lG60KS.png)

```py
def checkfile(filepath):
    with open(filepath,'r') as f:
        print(f.read())
checkfile('./file1')
checkfile('file1')
checkfile('test3/file5')
checkfile('../test2/file2')
checkfile('../test2/test4/file6')
checkfile('../file3')
```

### 1.2.1. 参考
1. <a href = "https://www.cnblogs.com/wuliytTaotao/p/9338259.html">Python相对路径</a>
