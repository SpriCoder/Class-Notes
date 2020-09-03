<!-- TOC -->

- [1. 职责(功能)](#1-职责功能)
- [2. 世界观的变化](#2-世界观的变化)
- [3. 对象(职责的实现)](#3-对象职责的实现)
- [4. 面向对象分析](#4-面向对象分析)
- [5. 封装](#5-封装)
  - [5.1. 类的职责和封装](#51-类的职责和封装)
    - [5.1.1. 数据职责：](#511-数据职责)
    - [5.1.2. 行为职责:](#512-行为职责)
- [6. GC](#6-gc)
- [7. Supplier](#7-supplier)
  - [7.1. 源码](#71-源码)
  - [7.2. 测试说明代码](#72-测试说明代码)
  - [7.3. 对于有初始化参数的复杂对象的装配技巧](#73-对于有初始化参数的复杂对象的装配技巧)
  - [7.4. 如果包含了含参的构造方法](#74-如果包含了含参的构造方法)
  - [7.5. 参考](#75-参考)

<!-- /TOC -->
**面向对象编程(职责&协作)**
1. 对象定义：是类的实例，有具体的属性，可以执行具体的行为。    
# 1. 职责(功能)
1. 职责：可以理解为对象的功能
    + 通过对需求的分析，寻找系统所需的功能，然后通过设计确定每个类的职责。
    1. 单一职责原则：(SRP):就一个类而言，应该仅有一个引起它变化的原因。
    2. 需求：是系统必须具有的特征，或者是客户可接受的、系统必须满足的约束，即我们要解决的“问题”。
    2. 场景：一种人们将做什么的陈述性描述，以及人们试图利用计算机系统和应用程序经验的陈述性描述。描述一个用户和系统之间交互的一系列步骤。场景是需求获取中的一种重要手段。一个场景是来自单一参与者的、具体的、关注点集中的系统单一特征的非形式化描述。
2. 数据职责和行为职责在一起，将职责合理分配
3. 数据和行为之间存在于一定的关系
4. 类和对象：
   1. 类-职责的抽象
   2. 对象-职责的实现
   3. 类和对象是抽象与具体关系
5. 给对象分配责任的策略：
    1. 分配所有重要方面
    2. 寻找需要执行的动作以及需要为何和生成的信息
6. **数据职责是方法要用到的数据**
7. 行为职责

# 2. 世界观的变化
1. 想法的变化：从原来函数之间的调用到现在的有职责的对象之间的交互
2. 视角的变化：
    1. 行为视角：结构化方法
    2. 数据视角：数据为中心方法
    3. 职责方法：面向对象方法
3. 在这种思路下，我们使用类来描述对象，使用类的方法来定义他们的行为

# 3. 对象(职责的实现)
1. 对象是什么：
    1. 定义：
        1. 是客观世界问题空间中的某个具体的事物
        2. 是软件系统解空间中的基本元素
    2. 对象的特征：
        1. 每个对象都保存着描述当前特征的信息
        2. 对象状态的改变必须通过调用方法来实现
        3. 每个对象都有唯一的身份，边坡是永远不同，状态也常常不同
2. 对象的初始化:
   1. 在方法甚至是构造方法之前初始化
   2. 优先初始化静态变量，静态变量按照文字顺序初始化,例子见pdf
3. 对象定义：是类的实例，有具体的属性，可以执行具体的行为。  
# 4. 面向对象分析
1. CRC卡  <a href = "https://baike.baidu.com/item/CRC卡/5702018?fr=aladdin">详见</a>
2. 类图
3. UMC：统一化建模语言
4. 用例分析

# 5. 封装
1. 封装三原则：
   1. 数据和行为放置在一块（在一起）
   2. 使用职责驱动型设计来决定如何确定一个类中的数据和行为
   3. 职责应当具有完备性
2. 是防御性编程的范例之一
3. 封装的作用：
    1. 隐藏类的实现细节，将类对外提供服务的接口和内部的具体实现进行分离，有助于在不影响对类使用的情况下修改类内部的实现
    2. 控制对成员变量的访问（可以在对成员变量的访问中加入数据检查和控制逻辑，限制对成员变量不合理的操作）

## 5.1. 类的职责和封装
1. 责任是指对象持有、维护特定数据并基于该数据进行操作的能力。
2. 面向对象三要素：
    1. 封装
    2. 继承
    3. 多态
### 5.1.1. 数据职责：
   + 表征对象的本质特征
   + 行为（计算）所需要的数据
### 5.1.2. 行为职责:
   + 表征对象的本质行为
   + 拥有数据所应该体现的行为

# 6. GC
1. 使用`finalize()`
2. 当垃圾回收器将要回收对象所占用内存之前被调用，也就是当对象被宣告死亡时先调用你finalize()方法，用来处理对象的所有事情。
3. 但是这个方法有着极大不确定性。

# 7. Supplier
1. 一种创建对象的方式,换句话说，这为我们提供了延迟对对象进行实例化。
2. 最大的特点就是lazy-init

## 7.1. 源码
```java
package java.util.function;

/**
 * Represents a supplier of results.
 *
 * <p>There is no requirement that a new or distinct result be returned each
 * time the supplier is invoked.
 *
 * <p>This is a <a href="package-summary.html">functional interface</a>
 * whose functional method is {@link #get()}.
 *
 * @param <T> the type of results supplied by this supplier
 *
 * @since 1.8
 */
@FunctionalInterface
public interface Supplier<T> {
    /**
     * Gets a result.
     *
     * @return a result
     */
    T get();
}
```

## 7.2. 测试说明代码
```java
public class TestSupplier {
	private static int age = 0;
	TestSupplier(){
		System.out.println(age);
	}

	public static void main(String[] args) {
		//创建Supplier容器，声明为TestSupplier类型，此时并不会调用对象的构造方法，即不会创建对象
		Supplier<TestSupplier> sup= TestSupplier::new;
		System.out.println("--------");
		//调用get()方法，此时会调用对象的构造方法，即获得到真正对象
		sup.get();
		//每次get都会调用构造方法，即获取的对象不同
        age = 1;
		sup.get();
	}
}

//另一个实例
public static void main(String[] args){
    Supplier<String> supplier = () => "hello world";
    System.out.println(supplier.get())
}
```

## 7.3. 对于有初始化参数的复杂对象的装配技巧
```java
public class Student(){
    private Sring name = "cexo";
    private int age = 20;

    public Student(){}

    public String getName(){
        return name;
    }
}
//三种不同的初始化方式
public static void main(String[] args){
    //first
    Student student = new Student();
    System.out.println(student.getName());
    //second
    Supplier<Student> supplier = () -> new Student();
    System.out.println(supplier.get().getName());
    //third
    Supplier<Student> supplier = Student::new;
    System.out.println(supplier.get().getName());
}
```
1. 使用ctrl点击::时，会跳转到Supplier的定义，而如果点击new，则跳转到对应的类定义上去。

## 7.4. 如果包含了含参的构造方法
```java
public Student(name,age){
    this.name = name;
    this.age = age;
}
```
1. IDE会直接调用无默认方法的构造方法。

## 7.5. 参考
1. <a href = "https://blog.csdn.net/qq_28393323/article/details/81003964">supplier理解</a>