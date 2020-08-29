collections
---

<!-- TOC -->

- [1. namedtuple](#1-namedtuple)
- [2. 统计字符个数](#2-统计字符个数)
- [3. UserList](#3-userlist)
- [4. deque()](#4-deque)
- [5. ChainMap](#5-chainmap)
  - [5.1. new_child()](#51-new_child)
  - [5.2. parent()](#52-parent)
  - [5.3. maps](#53-maps)
- [6. OrderedDict](#6-ordereddict)
- [7. 参考](#7-参考)

<!-- /TOC -->

# 1. namedtuple
1. 是tuple的一种，但是更加强大
2. 不必通过索引值进行访问，而是通过名字进行访问不过其中的值是不可以被改变的
3. 其内部的值可以通过索引访问
```py
from collections import namedtuple

Animal = namedtuple('Animal','name age type')
# Point = namedtuple('Point',['x','y'])
perry = Animal(name = 'perry', age = 31, type = 'cat')
```

# 2. 统计字符个数
```py
from collections import Counter
c = Counter()
for ch in 'programming':
    c[ch] = c[ch] + 1
c
#Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
```
1. 更加便捷的方式:
```py
from collections import Counter
a = [1, 2, 3, 1, 1, 2]
result = Counter(a)
print result
```

# 3. UserList
1. class collections.UserList([list])
2. 模拟一个列表。这个实例的内容被保存为一个正常列表，通过 UserList 的 data 属性存取。实例内容被初始化为一个 list 的copy，默认为 [] 空列表。 list 可以是迭代对象，比如一个Python列表，或者一个 UserList 对象。
3. UserList 提供了以下属性作为可变序列的方法和操作的扩展:
4. data
   + 一个 list 对象用于存储 UserList 的内容。
5. 子类化的要求: UserList 的子类需要提供一个构造器，可以无参数调用，或者一个参数调用。返回一个新序列的列表操作需要创建一个实现类的实例。它假定了构造器可以以一个参数进行调用，这个参数是一个序列对象，作为数据源。
6. 如果一个分离的类不希望依照这个需求，所有的特殊方法就必须重写；请参照源代码进行修改。

# 4. deque()
方法|功能|备注
--|--|--
append(x)|添加x到右端|-
appendleft(x)|添加x到左端|-
clear()|清空|-
copy()|浅拷贝|-
count(x)|统计其中x的出现次数|-
extend(iterable)|右侧添加iterable对象中元素|-
extendleft(iterable)|左侧添加iterable对象中的元素|-
index()|查找对应元素第一次出现的index|-
insert()|插入|-
pop()|从右边弹出|-
popleft()|从左边弹出|-
remove(value)|移除对应value|-
reverse()|反转当前队列|-
rotate()|正数向右循环，负数向左循环|-
maxlen()|deque的最大长度|未设置为None

# 5. ChainMap
1. 是一种将多级字典链接起来，进行统一访问的数据结构
2. 注意:
   1. 是对于原来字典的指针，所以原来的字典修改会导致ChinaMap中的值的修改，反之亦然。
   2. 所有对于ChinaMap的增删改都是对于第一个字典的修改，而查则是从最高级向下查询直到找到
```
from collections import ChainMap
a = {"x":1, "z":3}
b = {"y":2, "z":4}
c = ChainMap(a,b)
print(c)
print("x: {}, y: {}, z: {}".format(c["x"], c["y"], c["z"]))

输出：
ChainMap({'x': 1, 'z': 3}, {'y': 2, 'z': 4})
x: 1, y: 2, z: 3
[Finished in 0.1s]
```

#
## 5.1. new_child()
1. 创建更高一级的空字典

## 5.2. parent()
1. 访问低一级的字段

## 5.3. maps
1. 会将串联起来的字典以列表的形式展示

# 6. OrderedDict
1. OrderDict是实现了对于字典对象中元素的排序的字典。
2. 顺序不同的内容一样的OrderedDict是相同的。
3. 例子如下:OrderedDict会将其作为两个不同的对象。
```py
import collections
print("Regular dictionary")
d={}
d['a']='A'
d['b']='B'
d['c']='C'
for k,v in d.items():
    print(k,v) 
print "\nOrder dictionary"
d1 = collections.OrderedDict()
d1['a'] = 'A'
d1['b'] = 'B'
d1['c'] = 'C'
d1['1'] = '1'
d1['2'] = '2'
for k,v in d1.items():
    print(k,v)
```
```
输出：
Regular dictionary
a A
c C
b B

Order dictionary
a A
b B
c C
1 1
2 2
```
4. 后期调整
```py
#按key排序
kd = collections.OrderedDict(sorted(dd.items(), key=lambda t: t[0]))
#按照value排序
vd = collections.OrderedDict(sorted(dd.items(),key=lambda t:t[1]))
```

# 7. 参考
1. <a href = "https://www.cnblogs.com/mangmangbiluo/p/9882097.html">python ChainMap的使用和说明s</a>
2. <a href = "https://www.cnblogs.com/notzy/p/9312049.html">Python中OrderedDict的使用</a>