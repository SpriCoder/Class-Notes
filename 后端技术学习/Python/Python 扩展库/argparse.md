argparse
---

# 1. argparse模块是啥
1. 涉及到从命令行直接读取参数的时候
```py
import argparse
parser = argparse.ArgumentParser()parser.add_argument("x", help="横坐标",type=int)parser.add_argument('y', help="纵坐标",type=int)args = parser.parse_args()
```

# 2. 函数

## 2.1. add_argument()函数
1. 可以看到上面这个函数中有三个参数，
2. 第一个是名称，就是为你读取的变量起一个名字，例如x,y，这个你随便起一个你自己要名字。
3. 第二个是help，这是提示信息，告诉你这个变量是啥东西，怎么看这些信息呢，在命令行输入：python no.py -h

## 2.2. optional arguments可选参数
```py
import argparse import argparse
parser = argparse.ArgumentParser()
parser.add_argument("x", help="横坐标",type=int)
parser.add_argument('y', help="纵坐标",type=int)
parser.add_argument("-v",help="x翻倍",action="store_true")
args = parser.parse_args()
if args.v:
  args.x=args.x*2
  x=args.x
  y=args.y
  print(x,y)
```
1. 如果加了参数 -v ，则x变为输入值的两倍。

# 3. 参考
1. <a href ="https://blog.csdn.net/explorer9607/article/details/82623591">argparse</a>
2. <a href ="https://www.cnblogs.com/dengtou/p/8413609.html">更多的参考</a>