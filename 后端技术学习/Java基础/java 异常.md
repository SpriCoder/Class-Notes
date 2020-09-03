<!-- TOC -->

- [1. 异常的产生和接收](#1-异常的产生和接收)
- [2. 多重异常](#2-多重异常)
- [3. 抛出异常到外部](#3-抛出异常到外部)
- [4. 异常的语法](#4-异常的语法)
- [5. 使用异常机制的建议](#5-使用异常机制的建议)
- [6. 创建自己的异常](#6-创建自己的异常)
- [7. 异常的消失](#7-异常的消失)
- [8. 一些异常实例](#8-一些异常实例)
- [9. java虚拟机中的实现](#9-java虚拟机中的实现)
- [10. java中遇到的部分异常](#10-java中遇到的部分异常)
  - [10.1. ExceptionInInitializerError异常](#101-exceptionininitializererror异常)
  - [10.2. ArithmeticException](#102-arithmeticexception)
  - [10.3. IOException](#103-ioexception)
  - [10.4. FileNotFoundException](#104-filenotfoundexception)
  - [10.5. 参考](#105-参考)

<!-- /TOC -->
1. 使用`try{}catch{}`来捕捉异常
    1. try告诉系统在哪一部分的代码可能有问题
    2. catch表示如果有异常的话如何处理
2. 在程序运行过程中出现的不能正常执行的情况称为异常
3. 异常：在程序运行过程中出现的不能正常执行的情况

# 1. 异常的产生和接收
1. 在一个方法中抛出一个异常
```java
public void takeRisk() throws BadException{
    throw new BadExcception();
}
```
2. 在一个地方接收这个异常
```java
public void crossFingers(){
    try{

    }catch(BadException ex){

    }
}
```
3. 可以抛出多个异常和接收多个异常
4. 多用于处理运行运行时错误
5. 可以在最后添加`finally{}`:内部的操作无论如何都会被执行

# 2. 多重异常
1. 异常是一种多态，是对于Exception类的继承
2. 多个异常累加的时候，我们从小到大放置
3. 程序按照文本顺序来读取

# 3. 抛出异常到外部
1. 如果一个方法内部并不知道如何处理一个异常，将它抛出去,throws
2. 程序内部抛出方法：throw
3. 实例：
```java
public void foo() throws ClothingException{
    
}
```

# 4. 异常的语法
1. 不能在try和catch之间有语句
2. 可以没有finally
    + finally子句中含有return语句：待查
3. java7之后处理异常的新特性
    1. 一个Catch语句可以捕获多个异常
        + `catch(Exception a|Exception b)`
    2. 更加精准的异常抛出
        + 精准抛出异常的例子：代码1-17:使用捕捉抛出再捕捉的方式
4. java7中的try-with-resources语句
    1. 实现了自动关闭相关资源的能力，见代码清单1-19和1-20
    2. 一个可以打开多个文件：`try(a;b)`

# 5. 使用异常机制的建议
1. 异常声明是API的一部分
2. 异常处理不能代替简单的测试
3. 不要过分地细化异常
4. 利用异常层次结构
    + 不要只抛出RuntimeException，应该寻找更合适的子类或者创建自己的异常类
5. 不要压制异常
6. 在检测错误时，苛刻比放任要好
    + 比如在错误的时候抛出EmptyStackException比抛出NullPointerException异常更好
7. 不要羞于传递异常
    + 早抛出，晚捕获

# 6. 创建自己的异常
1. 精心设计异常的层次结构
2. 异常类中包含有足够的信息
3. 异常与错误提示

# 7. 异常的消失
1. 代码清单1-9:两种解决方法

# 8. 一些异常实例
1. 使用异常包装技术
2. 使用国际化异常消息的异常类的基类
3. 继承自支持国际化异常消息的异常类的子类

# 9. java虚拟机中的实现
1. athrow指令
2. 红色部分是异常表，对应左侧行数

# 10. java中遇到的部分异常

## 10.1. ExceptionInInitializerError异常
>spring boot框架中controller中持有dao层对象可能导致静态初始化问题。

1. 静态初始化块中出现异常，往往会导致这个错误。
2. 迷惑:有时候我们没有使用java的静态初始化变量，但是其实类的成员变量也会被识别为静态初始化变量。

## 10.2. ArithmeticException
1. 数学计算异常:比如除以0

## 10.3. IOException
1. 属于I/O部分的异常

## 10.4. FileNotFoundException
1. 没有找到文件的异常

## 10.5. 参考
1. <a href = "https://blog.csdn.net/xie_xiansheng/article/details/50831623">Java中的ExceptionInInitializerError 异常解决方法</a>