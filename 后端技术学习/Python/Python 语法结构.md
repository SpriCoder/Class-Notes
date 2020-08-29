<!-- TOC -->

- [1. 第一个python程序](#1-第一个python程序)
  - [1.1. 运行方式](#11-运行方式)
  - [1.2. pypi](#12-pypi)
  - [1.3. print函数](#13-print函数)
  - [1.4. input()函数](#14-input函数)
  - [1.5. python风格](#15-python风格)
  - [1.6. 注释规范](#16-注释规范)
- [2. python基础语法](#2-python基础语法)
  - [2.1. 变量与常量](#21-变量与常量)
    - [2.1.1. 变量作用域](#211-变量作用域)
  - [2.2. 表达式](#22-表达式)
  - [2.3. 赋值](#23-赋值)
  - [2.4. 逗号运算符](#24-逗号运算符)
  - [2.5. 语句](#25-语句)
  - [2.6. 程序控制](#26-程序控制)
    - [2.6.1. 条件结构](#261-条件结构)
    - [2.6.2. 循环结构](#262-循环结构)
    - [2.6.3. for循环遍历两个列表失败](#263-for循环遍历两个列表失败)
    - [2.6.4. 循环控制](#264-循环控制)
- [3. python基本运算](#3-python基本运算)
- [4. 函数](#4-函数)
  - [4.1. 内置函数](#41-内置函数)
    - [4.1.1. sort()函数](#411-sort函数)
    - [4.1.2. map()函数](#412-map函数)
    - [4.1.3. eval()函数](#413-eval函数)
    - [4.1.4. bin()函数](#414-bin函数)
  - [4.2. python常用标准库函数](#42-python常用标准库函数)
    - [4.2.1. math模块中的函数](#421-math模块中的函数)
    - [4.2.2. os模块](#422-os模块)
    - [4.2.3. random模块](#423-random模块)
    - [4.2.4. datatime模块](#424-datatime模块)
    - [4.2.5. range()函数](#425-range函数)
  - [4.3. 自定义函数](#43-自定义函数)
    - [4.3.1. 默认参数](#431-默认参数)
    - [4.3.2. 关键字参数](#432-关键字参数)
    - [4.3.3. 传递函数](#433-传递函数)
    - [4.3.4. 匿名函数(lambda函数)](#434-匿名函数lambda函数)
- [5. 异常](#5-异常)
  - [5.1. 处理异常](#51-处理异常)
  - [5.2. else子句](#52-else子句)
  - [5.3. finally子句](#53-finally子句)
  - [5.4. 循环](#54-循环)
  - [5.5. 上下文管理器](#55-上下文管理器)
- [6. Lambda表达式](#6-lambda表达式)
  - [6.1. Lambda语法](#61-lambda语法)
  - [6.2. Lambda三个特性](#62-lambda三个特性)
  - [6.3. 四个用法](#63-四个用法)
- [7. yield的用法](#7-yield的用法)
  - [7.1. 以斐波那契数列为例解析yield](#71-以斐波那契数列为例解析yield)
  - [7.2. yield详解](#72-yield详解)
  - [7.3. send函数](#73-send函数)
  - [7.4. 判断是否为可迭代函数](#74-判断是否为可迭代函数)
  - [7.5. 判断是否是实例](#75-判断是否是实例)
  - [7.6. return的作用](#76-return的作用)
  - [7.7. 参考](#77-参考)
- [8. Python文件头的含义](#8-python文件头的含义)
- [9. 参考](#9-参考)

<!-- /TOC -->

# 1. 第一个python程序

## 1.1. 运行方式
1. Shell方式
2. IDE方式(文件方式)

## 1.2. pypi
1. 使用anaconde进行安装
2. 使用pip安装插件

## 1.3. print函数
1. python2不加括号
2. `print('x1 = {0(从零开始的对应后面参数位置的占位符):.1f}, x2 = {:.1f}'.format(x1, x2))`使用format来格式化输出
3. `print(format_string % (argument))`基本被替代的输出
4. `print(1,end=" ")`:结尾是空格
5. 格式说明符:详情需要查，以下是几个特殊的
    + `<`左对齐，默认情况右对齐
    + 两个大括号表示大括号
## 1.4. input()函数
1. 返回字符串
2. input(提示语)

## 1.5. python风格
1. #是注释
2. \是续航，可以用三引号来完成
3. 一行多语句，用分号
4. 缩进来表示代码块，常用4个空格

## 1.6. 注释规范
1. 我们使用跨行注释来为函数进行注释
```py
""" Summary of class
details

Args:
    参数说明

Returns:
    返回值描述

Raises:
    可能发生的异常
"""
```
2. <a href = "https://blog.csdn.net/lly1122334/article/details/80733908">Python类和方法注释规范</a>
# 2. python基础语法

## 2.1. 变量与常量
1. 变量名命名规则(表示符):和c一样
    1. 首字母:字母和下划线
    2. 其余:数字、字母和下划线
    3. 大小写敏感
2. 注:
    1. 我们通常认为全大写的变量是常量
    2. `_`开头的变量一般被认为是类的私有变量
    3. 尽量做到见名识义
    4. 避开关键字

### 2.1.1. 变量作用域
1. 变量出现在哪里，就作用在哪里
2. 全局变量:在程序主体部分
3. 局部变量:在函数部分
4. 全局变量和局部变量重名:内层屏蔽外层。
5. 不能局部，修改全局变量的值，如果需要修改，参考下面例子
```python
def f(x):
    global a
    a = 5
a = 3
f(8)
```

## 2.2. 表达式
1. 用运算符链接各种类型数据的式子就是表达式

## 2.3. 赋值
1. 变量第一次赋值，同时获得"类型"和"值"
2. Python是强类型语言
3. 不需要显式声明，以“引用”的方式实现赋值。
4. 支持"增量赋值"(+=)，支持链式赋值，从右先左赋值。支持多重赋值(`x,y = 1,2`)

## 2.4. 逗号运算符
1. 又叫元组运算符
2. `a,b = b,a+b`相当于元组赋值

## 2.5. 语句
1. 完整执行一个任务的一行代码。

## 2.6. 程序控制

### 2.6.1. 条件结构
1. if语句
2. else语句
3. elif语句
4. 条件嵌套

### 2.6.2. 循环结构
1. while语句
2. for循环
    1. 语法:`for x in iterable_object`
    2. iterable_object:
        1. String
        2. List
        3. Tuple
        4. Dictionary
        5. File
        
### 2.6.3. for循环遍历两个列表失败
1. 为何在`for v,t in _,_:`的时候会出现问题?
    1. 如果个数不匹配会出现问题。
    2. 应该保证相关部分均为
2. 你在后面写的时候`_,_`会生成一个tuple

### 2.6.4. 循环控制
1. break语句
2. continue语句
3. else语句:
    1. 如果循环代码从break处跳出时，不执行其中语句。
    2. 如果正常结束循环，执行其中循环

# 3. python基本运算
1. 算术运算:
    1. 乘方:`**`
    2. 整除:`//`
    3. 注解:
        + `-3**2 = -9`
        + `(-3)**2 = 9`
2. 比较运算:
    1. 按照值、ASCII码进行比较
    2. python允许写成和数学一样的式子:`3<4<7 equal to 3<4 && 4<7`
3. 逻辑运算:
    + and,or,not
4. 字符运算:
    1. 原始字符串操作符(r/R):用于一些不希望转义字符起作用的地方。
    2. 所有字符串都是Unicode字符串。
5. 综合运算
    1. 考虑优先级问题

# 4. 函数

## 4.1. 内置函数

### 4.1.1. sort()函数
```py
list.sort(key = None,reverse  = False) # 返回值为None
```
1. python3中的sort()函数往往是用于对原列表、字典进行排序。
2. 可以使用指定参数来保证排序的顺序以及指定的比较函数。
```py
def sortByFirst(nums):
    return nums[0]
# 根据二维链表的第一个元素进行排序
```

### 4.1.2. map()函数
```py
map(function,iterable,...)
```
1. 函数的参数解释，function是函数，iterable是一个或多个序列。
2. map函数常常是和lambda函数以及常用的内置函数。
```py
def square(x):
    return x ** 2
map(square,[1,2,3,4,5])
map(lambda x: x ** 2,[1,2,3])s
map(lambda x, y: x + y,[1,2,3],[4,5,6])
```
2. map函数常常可以结合lambda表达式，以及可以处理批量数组的输入输出。

### 4.1.3. eval()函数
1. eval()函数可以对一个string进行计算，来获取最后的结果。
2. eval()函数同样可以对二进制、十六进制字符串进行计算。
```py
string = "0b1010"
print(eval(string)) # 10
```
3. eval(input())可以将其输入转换成python的内置数据格式。

### 4.1.4. bin()函数
1. bin()函数完成了将整数转化成为二进制的字符串
2. 此外将整数转化为二进制字符串的方法还有:
    1. 使用自己编写函数，按照二进制的定义予以完成。
    2. 使用bin()函数进行实现，但是返回的字符串会有`0b`前缀
    3. 使用format方法来获取二进制。`{0:b}.format(123456)`

## 4.2. python常用标准库函数

### 4.2.1. math模块中的函数
1. 导入模块:`import math`
2. 查看包含函数:`dir(math)`
3. 查看包含常数的值:`math.pi`
4. 查看函数功能:`help(math.exp)`
5. 内置库函数:
    1. ceil(x)向上取整。
    2. floor(x)向下取整
    3. pow(x,y)求x的y次方
    4. sqrt(x)求x的平方根
    5. degrees(x):弧度转角度
    6. radians(x):角度转弧度

### 4.2.2. os模块
1. 包含和操作系统交互的函数。
2. 导入模块
3. 具体函数:
    1. os.getcwd()获取当前工作目录
    2. os.chdir(string)调整工作路径
    3. os.rename(past_name,new_name)更改文件名称
    4. os.remove(name)删除掉文件

### 4.2.3. random模块
1. 这个模块提供了多个生成随机数的函数
2. 导入模块
3. 具体函数:
    1. random.choice([a,b,c])从序列中获取一个随机值
    2. random.randint(number1,number2)生成两个数之间的一个数
    3. random.randrange(number1,number2,step)从生成的数中获取一个随机整数
    4. random.random()生成一个0到1.0之间的一个随机浮点数，包含0但是不包含1.0
    5. random.uniform(number1,number2):生成两个数之间的一个浮点数
    6. random.sample(range(100),10)从给定的序列中获取给定个数的一组值，返回列表
    7. random.shuffle(list)将列表中的元素打乱，适合抽签。

### 4.2.4. datatime模块
1. 这个模块为日期和时间处理提供了各种方法，重点是为了输出格式化和处理。
2. 导入模块
3. 具体包含的模块:
    1. data模块:
        1. 导入模块:`from datatime import date`
        2. data.today()输出今天时间
    2. time模块
        1. 导入模块:`from datatime import time`
        2. time(小时,分钟,秒钟)用来创建一个时间
    3. datatime模块:
        1. 导入模块:`from datatime import datatime`
        2. datatime.now()来查看当前的时间，最后一个参数是毫秒
    4. strftime()函数
        1. 用来格式化一个时间
        2. 例子:
            + `dt = datatime.now`
            + `print(dt.strftime('%a, %b %d %Y %H:%M')`
            + result:Sun, Apr 30 2017 15:00
    5. dt.timestamp()函数
        1. 把一个datatime转化成全球统一的时间戳
    6. dt.fromtimestamp()函数
        1. 把时间戳转化为本地的日期和时间
4. 时间戳:1971年1月1日0时0时区的时刻称为新纪元时间，从新纪元时间到现在的毫秒数


### 4.2.5. range()函数
1. 语法:
    1. `range(start,end,step=1)`:步长为1，不含end的值。
    2. `range(start,end)`:步长默认为1
    3. `range(end)`:默认start为0，step=1
2. 返回值:range对象，可迭代
3. 返回的是一个生成器，不是直接生成所有结果，而是使用一个后计算下一个。
4. xrange()

## 4.3. 自定义函数
1. 语法格式:
```python
def function_name(arguments):
    action
```
2. 函数调用:用函数名进行调用+小括号

### 4.3.1. 默认参数
1. 函数的参数可以有一个默认值，如果有提供默认值，在函数定义中，默认参数以赋值语句的形式提供。
2. 默认参数可以不传递实参进入。
3. 默认参数后面不能有非默认参数。默认参数一般需要放置到参数列表后。

### 4.3.2. 关键字参数
1. 参数比较多，可以直接写参数名来赋值而不用考虑参数顺序。即允许改变参数顺序。
2. 使用了关键字参数，那么之后必须全为关键字参数。

### 4.3.3. 传递函数
1. 可以传递函数
2. 实例
```python
def addMe(x):
    return (x+x)
def self(f,y):
    print(f(y))
self(addMe,2)
```

### 4.3.4. 匿名函数(lambda函数)
1. 例子:
```python
r = lambda x:x+x
r(5)
```
2. 不需要写函数名，更加精简

# 5. 异常
1. python使用异常对象来表示异常。
2. 未被捕捉的时候，通过回溯来完成查找
3. 查看异常类的指令:`dir(_builtins_)`

## 5.1. 处理异常
1. try-except语句
```python
try:
    raise
except Exception as err:
    print(err)
```
2. 多个except子句捕捉异常
```python
try:
    num1 = int(input("Please input the first number"))
    num2 = int(input("Please input the second number"))
    print(num1/num2)
except ValueError:
    print("1")
except ZeroDivisionError:
    print("2")
```
3. 使用一个except块捕捉多个异常
```python
try:
    num1 = int(input("Please input the first number"))
    num2 = int(input("Please input the second number"))
    print(num1/num2)
except(ValueError,ZeroDivisionError):
    print("Bad!")
```
4. 空的except捕捉所有异常

## 5.2. else子句
1. 如果没有异常被触发，那么执行else内部的语句

## 5.3. finally子句
1. 无论是否发生了异常，都会执行finally子句中的部分

## 5.4. 循环
1. 小技巧，通过循环做到，输入错误可以继续输入
```python
while True:
    try:
        num1 = int(input("Please input the first number"))
        num2 = int(input("Please input the second number"))
        print(num1/num2)
        break
    except ValueError:
        print("1")
    except ZeroDivisionError:
        print("2")
```

## 5.5. 上下文管理器
1. 定义和控制代码块执行前的准备动作及执行后的收尾动作。
```python
with open('data.txt') as f:
    for line in f:
        print(line.end=" ")
```
2. with语句常用于操作文件和数据库的时候

# 6. Lambda表达式
1. lambda不能包含命令，包含的表达式不能超过一个

## 6.1. Lambda语法
语法:`lambda argument_list: expression`
1. lambda是Python预留的关键字。
2. argument_list:是参数列表，由自己定义。
3. expression:是一个关于参数的表达式。表达式中出现的参数需要在argument_list中有定义，并且表达式只能是单行的。

## 6.2. Lambda三个特性
1. lambda函数是匿名的
2. lambda函数有输入和输出
3. lambda函数一般功能简单

## 6.3. 四个用法
1. 将lambda函数赋值给一个变量，通过这个变量间接调用该lambda函数。
```py
p = lambda x,y: x + y
p(4,6)# 10
```
2. 将lambda函数赋值给其他函数，从而将其他函数用该lambda函数替换。
3. 将lambda函数作为其他函数的返回值，返回给调用者。
4. 将lambda函数作为参数传递给其他函数。
    + filter函数:此时lambda函数用于指定过滤列表元素的条件。
    + sorted函数：用于对列表中所有元素进行排序的准则。
    + map函数:此时lambda函数用于指定对列表中每一个元素的共同操作。
    + reduce函数:此时lambda函数用于指定列表中两两相邻元素的结合条件。例如reduce等

# 7. yield的用法
1. yield的返回值是一个**生成器(generator)**的一部分。
    + 就是进行一个循环处理后，停留在yield一行。

## 7.1. 以斐波那契数列为例解析yield
1. 常规编程
```py
#!/usr/bin/python
# -*- coding: UTF-8 -*-
def fab(max): 
    n, a, b = 0, 0, 1 
    while n < max: 
        print b 
        a, b = b, a + b 
        n = n + 1
fab(5)
```
2. 提高复用性:使用链表存储提高复用性(空间换取时间)
```py
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
def fab(max): 
    n, a, b = 0, 0, 1 
    L = [] 
    while n < max: 
        L.append(b) 
        a, b = b, a + b 
        n = n + 1 
    return L
 
for n in fab(5): 
    print n
```

3. 通过iterable对象来迭代:
```py
for x in range(1000):pass
for x in xrange(1000):pass # 返回迭代器对象 py2
# python3 取消了xrange，xrange和range相同
```
4. 第三个版本:使用类持有
```py
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
class Fab(object): 
 
    def __init__(self, max): 
        self.max = max 
        self.n, self.a, self.b = 0, 0, 1 
 
    def __iter__(self): 
        return self 
 
    def next(self): 
        if self.n < self.max: 
            r = self.b 
            self.a, self.b = self.b, self.a + self.b 
            self.n = self.n + 1 
            return r 
        raise StopIteration()
 
for n in Fab(5): 
    print n
```

5. 第四个版本:使用yield，保持第一版的简洁性并且获得iterable的效果。
```py
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
def fab(max): 
    n, a, b = 0, 0, 1 
    while n < max: 
        yield b      # 使用 yield
        # print b 
        a, b = b, a + b 
        n = n + 1
 
for n in fab(5): 
    print n
```

## 7.2. yield详解
1. yield的作用就是一个函数变成一个generator，python解析器会把它视为一个迭代器。
```py
>>>f = fab(5) 
>>> f.next() 
1 
>>> f.next() 
1 
>>> f.next() 
2 
>>> f.next() 
3 
>>> f.next() 
5 
>>> f.next() 
Traceback (most recent call last): 
 File "<stdin>", line 1, in <module> 
StopIteration
```

## 7.3. send函数
1. send是发送一个参数给yield位置

## 7.4. 判断是否为可迭代函数
```py
from inspect import isgeneratorfunction 
sgeneratorfunction(fab)
```

## 7.5. 判断是否是实例
```py
isinstance(fab, types.GeneratorType)
```

## 7.6. return的作用
1. 另一个 yield 的例子来源于文件读取。如果直接对文件对象调用 read() 方法，会导致不可预测的内存占用。好的方法是利用固定长度的缓冲区来不断读取文件内容。通过 yield，我们不再需要编写读文件的**迭代类**，就可以轻松实现文件读取：
```py
def read_file(fpath): 
    BLOCK_SIZE = 1024 
    with open(fpath, 'rb') as f: 
        while True: 
            block = f.read(BLOCK_SIZE) 
            if block: 
                yield block 
            else: 
                return
```

## 7.7. 参考
1. <a href = "https://blog.csdn.net/mieleizhi0522/article/details/82142856">yield关键字</a>
2. <a href = "https://www.runoob.com/w3cnote/python-yield-used-analysis.html">Python浅析yield关键字</a>

# 8. Python文件头的含义
1. 在Linux\Mac上，可以用./文件路径直接运行.py文件，这时，需要在python文件开头指定解释器及其路径
```py
#! /usr/bin/python
```
2. 在Linux\Mac上，需要在py文件开头指定解释器及其路径
3. 使用编码格式
```py
# -*- coding: utf-8 -*-
```

# 9. 参考
1. <a href = "https://www.cnblogs.com/relex/p/11008944.html">python文件头的含义</a>