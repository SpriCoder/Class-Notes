<!-- TOC -->

- [1. 数据类型](#1-数据类型)
- [2. 列表](#2-列表)
  - [2.1. 列表解析](#21-列表解析)
  - [2.2. 生成器表达式](#22-生成器表达式)
- [3. 序列](#3-序列)
  - [3.1. 共性内容](#31-共性内容)
  - [3.2. 字符串](#32-字符串)
    - [3.2.1. 字符串的表示](#321-字符串的表示)
    - [3.2.2. 对字符串操作](#322-对字符串操作)
    - [3.2.3. 转义字符](#323-转义字符)
    - [3.2.4. eval()](#324-eval)
    - [3.2.5. 判断字符串形式](#325-判断字符串形式)
    - [3.2.6. python字符和ascii码之间的转换](#326-python字符和ascii码之间的转换)
    - [3.2.7. 完成字符串和数字的拼接](#327-完成字符串和数字的拼接)
    - [3.2.8. JSON和Dict数据格式之间的相互转换](#328-json和dict数据格式之间的相互转换)
  - [3.3. 列表list](#33-列表list)
    - [3.3.1. 独有的方法](#331-独有的方法)
    - [3.3.2. 列表解析](#332-列表解析)
  - [3.4. 元组](#34-元组)
    - [3.4.1. 特有函数](#341-特有函数)
    - [3.4.2. 元组用处](#342-元组用处)
    - [3.4.3. 生成元组的方法(zip)](#343-生成元组的方法zip)
  - [3.5. 字典](#35-字典)
    - [3.5.1. 什么是字典](#351-什么是字典)
    - [3.5.2. 创建字典](#352-创建字典)
    - [3.5.3. 函数处理](#353-函数处理)
    - [3.5.4. 使用字典](#354-使用字典)
    - [3.5.5. 字典的相关使用](#355-字典的相关使用)
  - [3.6. 集合](#36-集合)
    - [3.6.1. 创建集合](#361-创建集合)
    - [3.6.2. 集合比较](#362-集合比较)
    - [3.6.3. 集合的关系运算](#363-集合的关系运算)
    - [3.6.4. 内建函数](#364-内建函数)
- [4. 递归](#4-递归)
  - [4.1. 递归停止条件(递归出口)](#41-递归停止条件递归出口)
  - [4.2. 特点](#42-特点)
  - [4.3. 典型例子](#43-典型例子)
- [5. 图结构](#5-图结构)
  - [5.1. 有向图](#51-有向图)
    - [5.1.1. 查找一条路径、全部路径、最短路径](#511-查找一条路径全部路径最短路径)
  - [5.2. 无向图](#52-无向图)
- [6. 栈](#6-栈)
  - [6.1. 栈的列表实现](#61-栈的列表实现)
  - [6.2. 栈的链表实现](#62-栈的链表实现)
  - [6.3. 参考](#63-参考)

<!-- /TOC -->
# 1. 数据类型
1. 整形:
    1. 整形和长整型并不严格区分
    2. Python 2支撑整形值后加L即为长整型
    3. 几乎没有溢出问题
2. 布尔型:
    1. 整形的子类
    2. 仅有两个值：True、False
    3. 实际存储为1和0
3. 浮点型:
    1. 就是数学中的实数
    2. 可以类似科学计数法来表示
    3. type()可以查看类型
4. 复数型:
    1. j = 根号下-1,
    2. 实数加虚数就是复数
    3. 虚数部分必须有j，可以为0
    4. 可以分离实数部分和虚数部分
        1. 复数.real
        2. 复数.imag
    5. 复数中的共轭:复数.conjugate()
5. 序列类型:
    1. 字符串:不可变类型
        1. 使用单引号，双引号，三引号(三个单引号，可以自由使用单引号和双引号，可以换行)等
    2. 列表:可变类型，使用方括号
    3. 元组:不可变类型，使用圆括号
6. 映射类型:字典
    1. 使用大括号进行界别
    2. 类似哈希表的键值对

# 2. 列表
1.[]进行标识。

## 2.1. 列表解析
1. `[i for i in range(10)]`
2. `[i+1 for i in range(10) if i%2 == 0]`
3. 数据量比较小时使用。

## 2.2. 生成器表达式
1. `(i+1 for i in range(10) if i%2 == 0)`:返回一个生成器
2. 数据量比较大时使用。

# 3. 序列
1. 索引:和序列位置中相关的序号。
    + 索引:0-(N-1)/(-N)-(-1)

## 3.1. 共性内容
1. 可迭代:切片(**右边不取**)
    + [::-1]原序列逆序
2. 标准类型运算符:
    1. 值比较:字符串比较`<|>|<=|>=|==|!=`
    2. 对象身份比较`is|is not`
    3. 布尔运算`not|and|or`
3. 序列类型运算符
    1. 获取`[::-1]`
    2. 重复`apple*3 = appleappleapple`
    3. 连接`+`
    4. 判断`in`
4. 内建函数
    1. 序列类型转换
        1. list():`list('hello')`
        2. str()
        3. tuple()
    2. 序列其他内建函数
        1. enumerate()：返回一个enumerate对象
        2. len()：字符串长度
        3. max()：求最大值
        4. min()：求最小值
        5. reversed()：逆序
        6. sorted()：排序
        7. sum()：求和
        8. zip()：把对象打包成一个相应序列

## 3.2. 字符串

### 3.2.1. 字符串的表示
1. 单引号''括起来，也可以用双引号""来表示
2. 三引号表示:是长字符串，并且单引号和双引号不用处理
3. 前面加r表示原始字符串

### 3.2.2. 对字符串操作
1. 连接:+
2. 输出参考print
3. 几个函数:
    1. join()方法:`str`.join(list)：把list中每一项用str连接起来
    2. find(str,firstindex,lastindex):查找字符串首字母所在位置，如果没找到返回-1
    3. lower():转换为小写，不对原函数操作
    4. split(str):按照str来切断
        + split()只能指定一个分割符，而需要进行多个字符切割需要引入re模块。
        + 可以通过引入re模块来多字符切割<a href = "https://blog.csdn.net/lucky_ricky/article/details/78202572">参考</a>
    5. replace(str1,str2):将str1替换成str2
    6. encode('(utf-8)'):对字符串进行编码
    7. decode():对字符串进行解码
    8. istitle():
    9. index(str,firstindex,lastindex):查找一个字符的位置
    10. get(number,[char]):获得number位置上的字符，如果有char的话，将相应位置上换成char

### 3.2.3. 转义字符
\:续行符

### 3.2.4. eval()
1. `eval('123,1234')`
>`(123,1234)`

### 3.2.5. 判断字符串形式
1. 正则表达式
2. 判断是否全是字母：isalpha()
    + 不区分大小写
3. 判断是否全是数字：isdigit()
4. 判断是否字母数字混合：isalnum()

### 3.2.6. python字符和ascii码之间的转换
1. ASCII码转换为数值，使用`ord(char)`
2. 数值转换为ASCII码，使用`chr(int)`

### 3.2.7. 完成字符串和数字的拼接
1. 为了避免报错，我们需要使用内置函数str()
2. 使用`,`运算符也可以用来打印数据，不用进行转换。
3. 我们还可以用%s和%d等格式化符号，注意
4. 问题:TypeError: not enough arguments for format string
5. 原因，应该将%后面的参数用括号括起来
```py
def a(x,y):
	print("%s : %s "%(x,y))
```

### 3.2.8. JSON和Dict数据格式之间的相互转换
1. 标准模块:`import json`

方法名|方法功能
--|--
loads()|将json数据转化为dict数据
dumps()|将dict数据转化为json数据
load()|读取json文件数据，转成dict数据
dump()|将dict数据转化为json数据后写入json文件

## 3.3. 列表list
1. 中括号[]括起来。
2. 可扩展的，可变化的容器对象常使用。
    + 可以包含任何对象:容器对象。
    + 可以包含不同类型的对象

### 3.3.1. 独有的方法
1. list.sort(Key = None,reverse = false):改变list，排序
    + 按长度解析，key = len
2. sorted(list):不改变list
3. list.pop()除去元素
4. list.append()添加元素
5. sum(list)求和
6. len(list)长处
7. list1.extend(list2):连接列表
8. enumerate(week):生成可迭代对象
9. reverse():逆序

### 3.3.2. 列表解析
1. 改变列表而不是新建列表的时候使用
2. 语法格式:
```python
[expression for expr in sequence1
            for expr2 in sequence2...
            for exprN in sequence3
            if condition]
```

## 3.4. 元组
1. 圆括号()括起来
2. 元组是不可变的，可以计算长度，可以切片，可以使用相关常数

### 3.4.1. 特有函数
1. sorted(aList):进行排序，未改变原始列表
2. aList.sort():**没有不存在**

### 3.4.2. 元组用处
1. 在映射类型中当做键来使用
2. 函数的特殊类型的参数
    1. `def foo(args1,*argst)`
    2. `foo(1,2,3):argst = (2,3)`
3. 函数的特殊返回值

返回对象个数|返回类型
0|None
1|object
2|tuple

### 3.4.3. 生成元组的方法(zip)
1. 通过zip函数，将可迭代对象中相应的元素打包成一个个元组，然后返回这些元组组成的列表

## 3.5. 字典
1. 建立对象之间的映射关系。

### 3.5.1. 什么是字典
1. 键(key)
2. 值(value)
3. key-value对

### 3.5.2. 创建字典
1. 直接创建:`{'a':100}`
2. dict函数:`dict([['a',100]])/dict(list)/dict(a=100)`
    + 只要存在明显的映射关系。
3. 生成字典:`dict(zip(list1,list2))`
    + zip是常用的打包方法。

### 3.5.3. 函数处理
1. `.fromkeys((keys),number)`：将所有的keys都映射为number
2. `sorted(字典)`:返回key的排序
3. `len(dict)`:返回个数
4. `hash(dict)`：判断一个对象是不是可以哈希的，如果可以则返回其哈希值。
5. `dict.keys()`:返回字典所有的key
6. `dict.values()`:返回字典的所有value
7. `dict.items()`:将列表中的key和value组合成元组返回。
8. `dict1.update(dict2)`:将dict2中的更新进入dict1中去
9. `dict.get(key[,str])`:访问元素的比较好的方法，不会发生异常，没有返回None(**如果有str，则为查找到相应元素，则返回str**)
10. `clear()`:删除字典，包括所有访问这个字典的变量(彻底删除)
11. `dict.pop(key)`:弹出一个对象

### 3.5.4. 使用字典
1. 键值查找`info['a']`
2. 更新`info['a']=1`
3. 添加`info['b']=2`
4. 成员判断`'a' in info`
5. 删除字典成员`del info['a']`

### 3.5.5. 字典的相关使用
1. JSON格式
2. 搜索引擎关键词查询
    1. 一般搜索引擎查询:
        1. `www.baidu.com/s?wd=%s`
        2. `cn.bing.com/search?q=%us`
    2. 实例如下
```python
import requests
kw = {'q':'python'}
r = requests.get('http;//cn.bing.com/search',params = kw)
print(r.text)#就是我们在搜索引擎中的查询
```
3. 字典作为可变长的关键字参数
    1. 元组:可边长位置参数:形参部分一个*
    2. 字典:可变长的关键字参数:形参部分两个**

## 3.6. 集合
1. 本身无序，不允许重复元素出现。
2. 分类:
    1. 可变集合
    2. 不可变集合

### 3.6.1. 创建集合
1. 使用set(list)函数:创建可变集合
2. 使用frozenset(list)函数:创建不可变集合

### 3.6.2. 集合比较

数学符号|python符号
--|--
属于|in
不属于|not in
等于|==
不等于|!=
包含于|<
真包含于|<= 

### 3.6.3. 集合的关系运算
1. 交集:`x & y`
2. 并集:`x | y`
3. 差集:`x - y`属于前面不属于后面
4. 不同时属于两个集合的成员:`x ^ y`
5. 计算符号可复合

### 3.6.4. 内建函数
1. 面向所有集合:
    1. `issubset(t)`子集
    2. `issuperset(t)`超集
    3. `union(t)`并集
    4. `intersection(t)`交集
    5. `difference(t)`差集
    6. `symmetric_difference(t)`移除两个集合中都存在的元素
    7. `copy(t)`拷贝
2. 面向可变集合
    1. `update(t)`更新
    2. `intersection_update(t)`交集更新
    3. `difference_update(t)`差集更新
    4. `symmetric_update(t)`
    5. `add(obj)`添加
    6. `remove(obj)`移除，不存在会出错
    7. `discard(obj)`移除指定元素，但是不存在不会出错
    8. `pop()`
    9. `clear()`


# 4. 递归

## 4.1. 递归停止条件(递归出口)

## 4.2. 特点
1. 执行效率低，系统开销大。

## 4.3. 典型例子
1. 汉诺塔问题

# 5. 图结构
1. python的图结构一般是按照类进行存储，具体的存储方式包含数组、有序无序双向链表、字典等方式进行存储。
2. 图是非线性的数据结构，由顶点和边组成
3. 在图中，由顶点组成的序列成为路径
4. python中可以用字典的方式来创建图

## 5.1. 有向图
1. 图中的顶点是有序的

### 5.1.1. 查找一条路径、全部路径、最短路径
1. 相同的节点不会在返回的路径中出现两次或两次以上(无环)
2. 使用回溯技术
```python
# 找到一条从start到end的路径
def findPath(graph,start,end,path=[]):
    path = path +[start]
    if start == end:
        return path
    for node in graph[start]:
        if node not in path:
            newpath = findPath(graph,node,end,path)
            if newpath:
                return newpath
    return None

# 找到所有从start到end的路径
def findAllPath(graph,start,end,path =[]):
    path = path + [start]
    if start == end:
        return [path]
    
    paths = []
    for node in graph[start]:
        if node not in path:
            newpaths=findAllPath(graph,node,end,path)
            for newpath in newpaths:
                paths.append(newpath)
    return paths

# 查找最短路径
def findShortestPath(graph,start,end,path=[]):
    path = path + [start]
    if start == end:
        return path
    
    shortestpath = []
    for node in graph[start]:
        if node not in path:
            newpath = findShortestPath(graph,node,end,path)
            if newpath:
                if not shortestPath or len(newpath)<len(shortestPath):
                shortestPath = newpath
    return shortestPath
```


## 5.2. 无向图
1. 图中的顶点是无序的

# 6. 栈
1. 在python中我们处理栈，常常使用类的方法或者维护一个序列(物理结构)来完成，但是我们还是应该将其考虑为类才能有更多的逻辑含义。

## 6.1. 栈的列表实现
```py
class Stack(object):

    def __init__(self):
     # 创建空列表实现栈
        self.__list = []

    def is_empty(self):
    # 判断是否为空
        return self.__list == []
    def push(self,item):
    # 压栈，添加元素
        self.__list.append(item)

    def pop(self):
    # 弹栈，弹出最后压入栈的元素
        if self.is_empty():
            return 
        else:
            return self.__list.pop()

    def top(self): 
    # 取最后压入栈的元素
        if self.is_empty():
            return
        else:
            return self.__list[-1]
```

## 6.2. 栈的链表实现
1. 我们需要注意的是，在链表实现栈的情况下，我们会付出一定的查询代价，来弥补我们自己的空间节省。
```py
class Node(object):

"""节点的实现"""
    def __init__(self,elem):
         self.elem = elem
         self.next = None

class Stack(object):

    def __init__(self):
    """ 初始化，链表头head"""
        self.__head = None

    def is_empty(self):

    """ 初始化，链表头head"""
        return  self.__head is None

    def push(self,item):

    """ 压栈"""
        node = Node(item)
        node.next = self.__head
        self.__head = node

    def pop(self):
    """ 弹栈"""
        if self.is_empty():
            return 
        else:
            p = self.__head
            self.__head = p.next
            return p.elem
```

## 6.3. 参考
1. <a href = "https://www.jianshu.com/p/189ea3fc3084">Python中的栈</a>