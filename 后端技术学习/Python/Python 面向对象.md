Python面向对象
---
1. 目前比较公认的面向对象特征来自斯坦福大学的John Mitchell的"concepts in programming languages",包括动态绑定、抽象、子类多态性和继承。

<!-- TOC -->

- [1. 抽象](#1-抽象)
- [2. 继承](#2-继承)
  - [2.1. 子类](#21-子类)
    - [2.1.1. 子类的重载](#211-子类的重载)
    - [2.1.2. 私有属性和方法](#212-私有属性和方法)

<!-- /TOC -->
# 1. 抽象
1. 类:抽象出来描述了对象的特征。
    1. 如果类什么都没做，类则称为被作为名称空间(namespace)
    2. 类属性:属于一个类，在类别创建后被使用，可以在类中方法和主程序方法中更新，和实例无关
        + 外部调用:`class_name.attr`
        + 内部调用:`self.attr`
```python
class ClassName(odject):# 括号中是其超类，如果没有的话，就是object
    # 数据和行为
    count = 1 #类属性
    def setName(self,name):
        self.name = name
    def function(self):# 要求第一个参数必须是self，无对应
        action

```
2. 对象:封装，类的实例
    1. 实例:`dog = Dog()`
    2. 对象的初识话方法:`__init__(self,)`:Python自动调用的第一个方法，构造方法
3. 程序入口:`if__name__ == '__main__'`

# 2. 继承
1. 父类(基类)子类(派生类)
    + "is-a"关系
2. "has-a"关系

## 2.1. 子类
1. 定义:如果无继承则使用object
```python
class SubClassName(ParentClass1[,ParentClass2,...]):
    class_suite
```
2. 支持单继承和多继承

### 2.1.1. 子类的重载
1. 可以重写父类的相应方法:但是重写了初始化方法不会直接自动调用父类的初始化方法。

### 2.1.2. 私有属性和方法
1. 通过“访问控制符”来限定成员函数的访问
2. `__`双下划线:_var属性会被__classname_var替换，防止父类与子类中的同名冲突
3. `_`单下划线:防止模块属性被通过import进行加载