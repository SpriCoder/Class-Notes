Python 排序
---

# 1. 常规排序方法

## 1.1. sort方法
1. `list.sort()`的方式，在原本的列表上进行排序
2. 如上的方法返回一个None，并且性能相对于sorted更优
3. 这个只适用于列表

## 1.2. sorted方法
1. `sorted(list)`的方式，他返回一个新的排好序的列表
2. 适用于一切可迭代对象

# 2. 自定义排序

## 2.1. 自定义key函数
1. 从Python2.4开始支持
2. Key函数必须满足的特征，接受一个参数，然后返回一个用来排序的键，这种技术在排序上是非常快的
```py
# 对复杂对象排序时，使用索引作为排序的键
student_tuples = [
    ('john', 'A', 15),
    ('jane', 'B', 12),
    ('dave', 'B', 10),
]
print(sorted(student_tuples, key=lambda student: student[2]))   # sort by age
# [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
# 使用对象命名属性进行排序
class Student:
  def __init__(self, name, grade, age):
    self.name = name
    self.grade = grade
    self.age = age
  def __repr__(self):
    return repr((self.name, self.grade, self.age))

student_objects = [
    Student('john', 'A', 15),
    Student('jane', 'B', 12),
    Student('dave', 'B', 10),
]
print(sorted(student_objects, key=lambda student: student.age))   # sort by age
# [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

## 2.2. operator模块的函数
1. 使得创建访问器函数变得更快、更容易
2. operator模块的函数有`operator.itemgetter()`，`operator.attrgetter()`
3. Python 2.5之后新增了`operator.methodcaller()`

### 2.2.1. 改写未使用该模块的排序
```py
# 上面的例子使用这些函数
from operator import itemgetter, attrgetter
print(sorted(student_tuples, key=itemgetter(2)))
# [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
print(sorted(student_objects, key=attrgetter('age')))
# [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

### 2.2.2. 实现多级别排序
```py
print(sorted(student_tuples, key=itemgetter(1,2)))
# [('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]
print(sorted(student_objects, key=attrgetter('grade', 'age')))
# [('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]
```

### 2.2.3. 固定参数方法调用
>str.count()方法可以通过统计在每个消息中感叹号的数量，来计算消息的优先度
```py
from operator import methodcaller
messages = ['critical!!!', 'hurry!', 'standby', 'immediate!!']
print(sorted(messages, key=methodcaller('count', '!')))
# ['standby', 'hurry!', 'immediate!!', 'critical!!!']
```

### 2.2.4. 定义排序方向
1. 降序或者升序使用reverse字段来确定,如下是降序排列
```py
print(sorted(student_tuples, key=itemgetter(2), reverse=True))
```

### 2.2.5. 排序稳定性
1. 从Python2.2开始，排序都是稳定的，如果排序键相同，则排序前后的顺序是固定不变的
2. reverse参数以及保持排序的稳定性

### 2.2.6. 多键不同方向排序
1. 第一个键升序，第二个键降序:分成2部分来做即可
```py
s = sorted(student_objects, key=attrgetter('age'))     # sort on secondary key
print(sorted(s, key=attrgetter('grade'), reverse=True))       # now sort on primary key, descending
# [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

### 2.2.7. 浮点数排序
1. 对于浮点数的排序，使用`locale.strxfrm()`作为key函数，或者`locale.strcoll()`作为对比函数。

# 3. 参考
1. <a href = "https://www.cnblogs.com/harrymore/p/9460532.html">Python：如何排序（sort）</a>