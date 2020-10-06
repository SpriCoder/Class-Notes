re
---
1. 使用正则表达式形式。

<!-- TOC -->

- [1. re模块的方法](#1-re模块的方法)
  - [1.1. search(pattern,string,flags=0)](#11-searchpatternstringflags0)
  - [1.2. match(pattern,string,flags=0)](#12-matchpatternstringflags0)
  - [1.3. findall(pattern,string,flags=0)](#13-findallpatternstringflags0)
  - [1.4. split(pattern,string,maxsplit=0,flags=0)](#14-splitpatternstringmaxsplit0flags0)
  - [1.5. finditer(pattern, string, flags=0)](#15-finditerpattern-string-flags0)
  - [1.6. sub(pattern, repl, string, count=0, flags=0)](#16-subpattern-repl-string-count0-flags0)
  - [1.7. compile(pattern,flags=0)](#17-compilepatternflags0)
- [2. 正则表达式](#2-正则表达式)
- [3. 注意](#3-注意)
  - [3.1. findall的优先级查询](#31-findall的优先级查询)
  - [3.2. split的优先级查询](#32-split的优先级查询)
- [4. 正则表达式的使用实例](#4-正则表达式的使用实例)
  - [4.1. 删除除字母和数字之外的所有字符](#41-删除除字母和数字之外的所有字符)
    - [使用`re.sub()`函数](#使用resub函数)
    - [使用isalpha、isnumberic和join方法](#使用isalphaisnumberic和join方法)
    - [使用isalnum和join方法](#使用isalnum和join方法)
- [5. 参考](#5-参考)

<!-- /TOC -->

# 1. re模块的方法
```py
import re
ret = re.findall('a', 'eva egon yuan')  # 返回所有满足匹配条件的结果,放在列表里
print(ret) #结果 : ['a', 'a']

ret = re.search('a', 'eva egon yuan').group()
print(ret) #结果 : 'a'
# 函数会在字符串内查找模式匹配,只到找到第一个匹配然后返回一个包含匹配信息的对象,该对象可以
# 通过调用group()方法得到匹配的字符串,如果字符串没有匹配，则返回None。

ret = re.match('a', 'abc').group()  # 同search,不过在字符串开始处进行匹配
print(ret)
#结果 : 'a'

ret = re.split('[ab]', 'abcd')  # 先按'a'分割得到''和'bcd',在对''和'bcd'分别按'b'分割
print(ret)  # ['', '', 'cd']

ret = re.sub('\d', 'H', 'eva3egon4yuan4', 1)#将数字替换成'H'，参数1表示只替换1个
print(ret) #evaHegon4yuan4

ret = re.subn('\d', 'H', 'eva3egon4yuan4')#将数字替换成'H'，返回元组(替换的结果,替换了多少次)
print(ret)

obj = re.compile('\d{3}')  #将正则表达式编译成为一个 正则表达式对象，规则要匹配的是3个数字
ret = obj.search('abc123eeee') #正则表达式对象调用search，参数为待匹配的字符串
print(ret.group())  #结果 ： 123

import re
ret = re.finditer('\d', 'ds3sy4784a')   #finditer返回一个存放匹配结果的迭代器
print(ret)  # <callable_iterator object at 0x10195f940>
print(next(ret).group())  #查看第一个结果
print(next(ret).group())  #查看第二个结果
print([i.group() for i in ret])  #查看剩余的左右结果
```

## 1.1. search(pattern,string,flags=0)
1. pattern:正则表达式中的字符串或原生字符串表示
2. string:待匹配字符串
3. flags:正则表达式使用时的控制标记
4. 在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象。

## 1.2. match(pattern,string,flags=0)
1. pattern : 正则表达式的字符串或原生字符串表示
2. string : 待匹配字符串
3. flags : 正则表达式使用时的控制标记
4. 从一个字符串的开始位置起匹配正则表达，返回match对象

## 1.3. findall(pattern,string,flags=0)
1. pattern : 正则表达式的字符串或原生字符串表示
2. string : 待匹配字符串
3. flags : 正则表达式使用时的控制标记
4. 搜索字符串，以列表类型返回全部能匹配的子串

## 1.4. split(pattern,string,maxsplit=0,flags=0)
1. pattern : 正则表达式的字符串或原生字符串表示
2. string : 待匹配字符串
3. maxsplit: 最大分割数，剩余部分作为最后一个元素输出
4. flags : 正则表达式使用时的控制标记
5. 将一个字符串按照正则表达式匹配结果进行分隔，返回列表类型。

## 1.5. finditer(pattern, string, flags=0)
1. pattern : 正则表达式的字符串或原生字符串表示
2. string : 待匹配字符串
3. flags : 正则表达式使用时的控制标记
4. 搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象。

## 1.6. sub(pattern, repl, string, count=0, flags=0)
1. 在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串
2. pattern : 正则表达式的字符串或原生字符串表示
3. repl : 替换匹配字符串的字符串
4. string : 待匹配字符串
5. count : 匹配的最大替换次数
6. flags : 正则表达式使用时的控制标记
7. repl:可以是一个函数:使用正则`?P<value>`来捕获组

```py
# -*- coding: utf-8 -*-
import re
def mul(matched):
    intStr = matched.group("number");  # 123
    intValue = int(intStr)
    addedValue = intValue * 2;  # 234
    addedValueStr = str(addedValue)
    return addedValueStr

n = eval(input())
while(n != 0):
    n -= 1
    string = input().strip()
    ret = re.sub("(?P<number>\d+)", mul, string)
    print(ret)
```

8. 避免count和flags混淆，最好还是显式的写明
9. count是指处理前几个

## 1.7. compile(pattern,flags=0)
1. 将正则表达式的字符串形式编译成正则表达式对象
2. pattern : 正则表达式的字符串或原生字符串表示
3. flags : 正则表达式使用时的控制标记

# 2. 正则表达式
```py
# 下述代码中的弱密码:全是数字、符号、字母
# 中等密码:数字加上符号、数字加上字母、字母加入符号
# 强密码:三个混合
def password_level(password):
    weak = re.compile(r'^((\d+)|([A-Za-z]+)|(\W+))$')
    level_weak = weak.match(password)
    level_middle = re.match(r'([0-9]+(\W+|\_+|[A-Za-z]+))+|([A-Za-z]+(\W+|\_+|\d+))+|((\W+|\_+)+(\d+|\w+))+',password)
    level_strong = re.match(r'(\w+|\W+)+',password)
    if level_weak:
        print 'password level is weak',level_weak.group()
    else:
        if (level_middle and len(level_middle.group())==len(password)):
            print 'password level is middle',level_middle.group()
        else:
            if level_strong and len(level_strong.group())==len(password):
                print 'password level is strong',level_strong.group()
```

# 3. 注意

## 3.1. findall的优先级查询
```py
import re
ret = re.findall('www.(baidu|google).com', 'www.google.com')
print(ret)  # ['google']     这是因为findall会优先把匹配结果组里内容返回,如果想要匹配结果,取消权限即可
ret = re.findall('www.(?:baidu|google).com', 'www.google.com')
print(ret)  # ['www.google.com']
```
## 3.2. split的优先级查询
```py
ret=re.split("\d+","eva3egon4yuan")
print(ret) #结果 ： ['eva', 'egon', 'yuan']
ret=re.split("(\d+)","eva3egon4yuan")
print(ret) #结果 ： ['eva', '3', 'egon', '4', 'yuan']
#在匹配部分加上（）之后所切出的结果是不同的，
#没有（）的没有保留所匹配的项，但是有（）的却能够保留了匹配的项，
#这个在某些需要保留匹配部分的使用过程是非常重要的。
```

# 4. 正则表达式的使用实例

## 4.1. 删除除字母和数字之外的所有字符

### 使用`re.sub()`函数
```py
import re

string = "123vbsgwe@#$%#@$%#$"
print("初始化字符串:", string)
# 123vbsgwe@#$%#@$%#$
result = re.sub("[\W_]+", "", string)
print("替换后的字符串:", result)
# 123vbsgwe
```

### 使用isalpha、isnumberic和join方法
```py
string = "123vbsgwe@#$%#@$%#$"
print("初始化字符串:", string)
# 123vbsgwe@#$%#@$%#$

result = "".join(list([char for char in string
                if char.isalpha() of char.isnumberic()]))

print("替换后的字符串:", result)
# 123vbsgwe
```

### 使用isalnum和join方法
```py
string = "123vbsgwe@#$%#@$%#$"
print("初始化字符串:", string)
# 123vbsgwe@#$%#@$%#$

result = "".join(list([char for char in string
                if char.isalnum()]))

print("替换后的字符串:", result)
# 123vbsgwe
```

# 5. 参考
1. <a herf = "https://www.cnblogs.com/yaogua/p/7205401.html">python使用正则表达式判断密码强弱</a>
2. <a href = "https://www.cnblogs.com/python-xkj/archive/2018/06/26/9231624.html">python re库入门</a>
3. <a href = "https://blog.csdn.net/flower_517/article/details/88709337">Python入门教程-如何删除除字母和数字之外的所有字符</a>