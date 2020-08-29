AOP
---
1. AOP是Spring框架的一个重要内容

<!-- TOC -->

- [1. 什么是AOP](#1-什么是aop)
  - [1.1. 什么是切面](#11-什么是切面)
- [2. AOP的正式的相关概念](#2-aop的正式的相关概念)
  - [2.1. Aspect(切面)](#21-aspect切面)
  - [2.2. Joint point(连接点)](#22-joint-point连接点)
  - [2.3. Pointcut(切点)](#23-pointcut切点)
  - [2.4. Advice(增强)](#24-advice增强)
  - [2.5. Target(目标对象)](#25-target目标对象)
  - [2.6. Weaving(织入)](#26-weaving织入)
- [3. Logger](#3-logger)
  - [3.1. logger参考](#31-logger参考)

<!-- /TOC -->
# 1. 什么是AOP
1. AOP(aspect oriented programming):面向切面编程，也就是通过预编译的方法和运行期动态代理实现程序功能的统一维护的一项技术。
2. AOP是OOP的延续，本身作为Spring框架中的一个重要内容，是函数式编程的一种衍生范型。

## 1.1. 什么是切面
1. 编程中，对象与对象之间，方法与方法之间，模块与模块之间都是切面
2. 将重复的部分提取出来，进行实现，然后将其作为具体的实现注入到框架中去

# 2. AOP的正式的相关概念

## 2.1. Aspect(切面)
其声明类似于Java中的类声明，在Aspect中会包含一些Pointcut以及向对应的Advice

## 2.2. Joint point(连接点)
表明程序中有明确定义的点，典型的包括方法调用，对类成员的访问以及异常处理程序块的执行，自身可以嵌套Joint point
1. 多种类型:
    1. 构造方法的调用
    2. 字段的设置和获取
    3. 方法的调用
    4. 方法的执行：**spring支持**
    5. 异常的处理执行
    6. 类的初始化

## 2.3. Pointcut(切点)
表示一组joint point，这些joint point可以通过逻辑关系、统配、正则表达式等方式集中起来，定义了相应的Advice将要发生的位置。

## 2.4. Advice(增强)
Advice定义了在Pointcut里面定义的程序点具体要做的操作，它通过before、after和around来区别是在每个joint point之前还是之后还是代替其他代码
1. 类型:
    1. `before advice`:在join point前被执行的advice，除非出现异常，否则不能阻止join point的执行。
    2. `after return advice`:在一个join point正常返回后执行的advice
    3. `after throwing advice`:在一个join point抛出异常后执行的advice
    4. `after(final) advice`:无论一个join point是否异常都会被执行的advice
    5. `around advice`:在join point前后都会执行的advice，这个是最常见的
    6. `introduction`:为原来的对象增加新的属性和方法
## 2.5. Target(目标对象)
织入Advice的目标对象

## 2.6. Weaving(织入)
将Aspect和其他对象关联起来，创建Advice odject的过程

# 3. Logger
1. Logging框架写log步骤：
    1. 引入Logger类和Logger工厂类
    2. 声明Logger
    3. 记录日志


## 3.1. logger参考
<a href ="https://blog.csdn.net/zalan01408980/article/details/79653386">Logger打印日志</a>