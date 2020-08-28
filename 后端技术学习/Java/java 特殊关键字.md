<!-- TOC -->

- [1. 关键字书写顺序](#1-关键字书写顺序)
- [2. static关键字](#2-static关键字)
- [3. final关键字](#3-final关键字)
- [4. synchronized关键字](#4-synchronized关键字)
- [5. super关键字](#5-super关键字)
- [6. this关键字](#6-this关键字)
- [7. default](#7-default)
- [8. native关键字](#8-native关键字)
- [9. exit()关键字](#9-exit关键字)
- [10. abstract方法](#10-abstract方法)

<!-- /TOC -->
# 1. 关键字书写顺序
1. 访问权限、修饰符、数据类型、变量名
2. `public final static int a;`

# 2. static关键字
摘自<a href ="https://www.cnblogs.com/dolphin0520/p/3799052.html">博客园</a>  
>static的方法就是没有this的方法，在static内部不能调用非静态的方法，反过来是可以的。而且可以在没有创建任何对象的前提下，仅仅通过类本身来调用static方法。——《java编程思想》
1. static方法
    1. 静态方法不依附于任何的对象，所以在静态方法中也就不能访问任何的**非静态**成员方法和**非静态**成员变量，但是反过来就行。
    2. 静态方法只能使用静态变量
2. static变量
   1. 静态变量在内存中只有一个副本。只在第一次中加载中被初始化。不依赖于具体的对象。
   2. 静态变量只会在类初次加载时被初始化，而非静态的方法是在对象被创建的时候被初次初始化。
   3. static成员变量的初始化顺序是按照定义进行初始化的。
   4. 常用的例子是累计使用的计数器。
3. static代码块
   1. 常用于形成静态代码块来优化程序性能。
   2. 通常将只需要初始化一次的变量放置到静态代码块中,常常是初始化静态代码块
4. `staitc int[] a;`可以直接访问第一个元素，为0，但是非static不行，会编译错误

# 3. final关键字
>这个关键字所修饰的变量一旦被初始化就不可以再被修改
1. final修饰成员
   + 变量：相当于常量，不可以被修改
   + 对象
2. final方法
   + 使用final方法的原因：
      1. 将方法锁定，以预防任何继承类修改它的意义，保证行为稳定不变，并且不会被重载。
      2. 效率
3. final代码块
   + 如果你的类是一个final类型的类，其中的所有的成员变量都是final的
4. 为什么使用final：
   1. 方法锁定
   2. 效率
5. final的类:不能被继承

# 4. synchronized关键字
1. 保证你的操作是安全的

# 5. super关键字
1. 如果你在子类中重写了父类的方法，但是想要使用父类中未被重写的方法，那么使用super关键字来完成引用  
2. 几个实例
```java
class father{
    private int a;
    public father(int a){
        this.a = a;
    }
}
class son extends father{
    public son(int a){
        super(a);
    }
}
```
```java
class A{
      int i;
      A(int i){
            this.i=i*2;
      }
}
class B extends A{
      public static void main(String args[]){
            B b=new B(2);
      }
      B(int i){
            System.out.println(i);
      }
} //编译失败，需要A中的无参数构造方法
```
上面的题：  
1. 如果父类中没有构造方法，默认调用无参数的构造方法
2. 如果父类中有有参数的构造方法，那么必须在子类中调用哪个构造方法，使用super
3. 如果子类中的构造方法没有使用super()的话，编译器会自动帮你添加super()，也就是为什么会需要无参数构造方法

# 6. this关键字
1. 调用本类的其他的成员或者构造方法
2. 访问不到静态变量，可以访问到非静态的
    + 类名.进行引用是可以进行访问的

# 7. default 
1. default关键字只能在接口中使用(使用在switch中的default不能用在抽象类中)
2. 这个用于解决为借口添加新方法而又不会破坏已有方法的实现，为升级旧接口且保持向后兼容提供了途径
3. 默认方法的继承问题：可以重写，参见PPT
4. 接口继承行为发生冲突时的解决规则：
    1. 使用super来调用父类中的方法
    2. 当接口继承行为发生冲突时的另一个规则是，类的方法声明优先于接口默认方法，无论该方法是具体的还是抽象的。
5. 接口方法不能重写:Object类中的`equals、hashCode、toString`
6. 接口中的静态方法必须是public的，这个修饰符可以省略，但是static修饰符不可以被省略

# 8. native关键字
1. 表示之后可能是非java的成分

# 9. exit()关键字
1. System.exit(number);
    + number被赋值
2. 跳出程序

# 10. abstract方法
1. 非抽象类中不可以有抽象方法
2. 抽象类中可以有非抽象方法
3. 非抽象方法必须带body