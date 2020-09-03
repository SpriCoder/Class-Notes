java 反射
---
<!-- TOC -->

- [1. JAVA反射机制定义](#1-java反射机制定义)
- [2. 用途](#2-用途)
- [3. 反射机制的相关类](#3-反射机制的相关类)
  - [3.1. Class类](#31-class类)
    - [3.1.1. 获取类相关的方法](#311-获取类相关的方法)
    - [3.1.2. 获得类中属性相关的方法](#312-获得类中属性相关的方法)
    - [3.1.3. 类中方法相关的方法](#313-类中方法相关的方法)
    - [3.1.4. 类中其他重要的方法](#314-类中其他重要的方法)
  - [3.2. Field类](#32-field类)
  - [3.3. Methods类](#33-methods类)
  - [3.4. Constructor类](#34-constructor类)
  - [3.5. 实例](#35-实例)
    - [实例一](#实例一)
    - [实例二](#实例二)
    - [实例三:](#实例三)
  - [3.6. 参考](#36-参考)

<!-- /TOC -->
# 1. JAVA反射机制定义
1. JAVA反射机制是在运行状态中，对于任意一个类，都能够知道这个类的**所有属性和方法**；对于任意一个对象，都能够调用它的**任意方法和属性**；这种动态获取信息以及动态调用对象方法的功能称为**java语言的反射机制**。

# 2. 用途
1. 在日常的第三方应用开发过程中，我们面临某个类的某个成员变量、方法或是属性是私有的或者系统应用开发。
2. 解决方式:我们可以通过java的反射机制来获取所需私有成员或者方法。
3. 问题:反射不是所有情况下都适用的，存在以下情况:在层层调用后在最终返回结果的地方对应用的权限进行了校验，对于没有权限的应用的返回值使用**缺省值**，否则返回实际值起到保护用户的隐私目的。

# 3. 反射机制的相关类
类名|相关类
--|--
Class|代表类的实体，在运行的java应用程序中代表类和接口
Field|代表类的成员变量(类的属性)
Methods|代表类的方法
Constructor|代表类的构造方法

## 3.1. Class类
1. Class代表类的实体，在运行的Java应用程序中表示类和接口。在这个类中提供了很多有用的方法，这里对他们简单的分类介绍。

### 3.1.1. 获取类相关的方法
方法|用途
--|--
asSubclass(Class<U> clazz)|把传递的类的对象转换成代表其子类的对象
Cast|把对象转换成代表类或是接口的对象
getClassLoader()|获得类的加载器
getClasses()|返回一个数组，数组中包含该类中所有公共类和接口类的对象
getDeclaredClasses()|返回一个数组，数组中包含该类中所有类和接口类的对象
forName(String className)|根据类名返回类的对象
getName()|获得类的完整路径名字
newInstance()|创建类的实例
getPackage()|获得类的包
getSimpleName()|获得类的名字
getSuperclass()|获得当前类继承的父类的名字 getInterfaces()|获得当前类实现的类或是接口

### 3.1.2. 获得类中属性相关的方法
方法|用途
--|--
getConstructor(Class...<?> parameterTypes)|获得该类中与参数类型匹配的公有构造方法
getConstructors()|获得该类的所有公有构造方法
getDeclaredConstructor(Class...<?> parameterTypes)|获得该类中与参数类型匹配的构造方法
getDeclaredConstructors()|获得该类所有构造方法

### 3.1.3. 类中方法相关的方法
方法|用途
--|--
getMethod(String name, Class...<?> parameterTypes)|获得该类某个公有的方法
getMethods()|获得该类所有公有的方法
getDeclaredMethod(String name, Class...<?> parameterTypes)|获得该类某个方法getDeclaredMethods()|获得该类所有方法

### 3.1.4. 类中其他重要的方法
方法|用途
--|--
isAnnotation()|如果是注解类型则返回true
isAnnotationPresent(Class<? extends Annotation> annotationClass)|如果是指定类型注解类型则返回true
isAnonymousClass()|如果是匿名类则返回true
isArray()|如果是一个数组类则返回true
isEnum()|如果是枚举类则返回true
isInstance(Object obj)|如果obj是该类的实例则返回true
isInterface()|如果是接口类则返回true
isLocalClass()|如果是局部类则返回true
isMemberClass()|如果是内部类则返回true

## 3.2. Field类
1. Field代表类的成员变量(类的属性)
方法|用途
--|--
equals(Object obj)|属性与obj相等则返回true
get(Object obj)|获得obj中对应的属性值
set(Object obj, Object value)|设置obj中对应属性值

## 3.3. Methods类
1. Methods代表类的方法
方法|用途
--|--
invoke(Object obj, Object... args)|传递object对象及参数调用该对象对应的方法

## 3.4. Constructor类
方法|用途
newInstance(Object... initargs)|根据传递的参数创建类的对象

## 3.5. 实例

### 实例一
1. 详见参考中Book实例，以下为部分代码截取。
```java
public class Book{
    private final static String TAG = "BookTag";

    private String name;
    private String author;

    @Override
    public String toString() {
        return "Book{" +
                "name='" + name + '\'' +
                ", author='" + author + '\'' +
                '}';
    }
    public Book() {}
    private Book(String name, String author) {
        this.name = name;
        this.author = author;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getAuthor() {
        return author;
    }
    public void setAuthor(String author) {
        this.author = author;
    }
    private String declaredMethod(int index) {
        String string = null;
        switch (index) {
            case 0:
                string = "I am declaredMethod 1 !";
                break;
            case 1:
                string = "I am declaredMethod 2 !";
                break;
            default:
                string = "I am declaredMethod 1 !";
        }
        return string;
    }
}
```
2. 创建对象:
```java
Class<?> classBook = Class.forName("com.android.peter.reflectdemo.Book");
Object objectBook = classBook.newInstance();
Book book = (Book) objectBook;
book.setName("Android进阶之光");
book.setAuthor("刘望舒");
```
2. 反射私有的构造方法:
```java
Class<?> classBook = Class.forName("com.android.peter.reflectdemo.Book");
Constructor<?> declaredConstructorBook = classBook.getDeclaredConstructor(String.class,String.class);
declaredConstructorBook.setAccessible(true);
Object objectBook = declaredConstructorBook.newInstance("Android开发艺术探索","任玉刚");
Book book = (Book) objectBook;
```
3. 反射私有属性:
```java
Class<?> classBook = Class.forName("com.android.peter.reflectdemo.Book");
Object objectBook = classBook.newInstance();
Field fieldTag = classBook.getDeclaredField("TAG");
fieldTag.setAccessible(true);
String tag = (String) fieldTag.get(objectBook);
```
4. 反射私有方法:
```java
Class<?> classBook = Class.forName("com.android.peter.reflectdemo.Book");
Method methodBook = classBook.getDeclaredMethod("declaredMethod",int.class);
methodBook.setAccessible(true);
Object objectBook = classBook.newInstance();
String string = (String) methodBook.invoke(objectBook,0);
```

### 实例二
1. 结合工厂模式，使用反射生成java的相关类
```java
public static Instruction getInstr(int opcode) {
        try {
            System.out.println(opcode);
            String className = Opcode.opcodeEntry[opcode].split("_")[0];
            className = className.substring(0, 1).toUpperCase() + className.substring(1);
            System.out.println(className);
            Class clazz = Class.forName(PREFIX + className);
            return (Instruction) clazz.getDeclaredConstructor().newInstance();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
            System.out.println("No Such instruction, please add it to the instruction table!");
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
```

### 实例三:
1. 详见参考3
2. 注意构造函数列表的顺序是根据类定义中上下文熟顺序完成的。
```java
class  Person{
    private int age;
    private String name;
    public Person(){}
    public Person(int age, String name){
        this.age = age;
        this.name = name;
    }

    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
public class ReflectDemo {
    private static String path = "";

    /**
     * 为了看清楚Java反射部分代码，所有异常我都最后抛出来给虚拟机处理！
     * @param args
     * @throws ClassNotFoundException
     * @throws InstantiationException
     * @throws IllegalAccessException
     * @throws InvocationTargetException
     * @throws IllegalArgumentException
     * @throws NoSuchFieldException
     * @throws SecurityException
     * @throws NoSuchMethodException
     */

    public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException, IllegalArgumentException, InvocationTargetException, SecurityException, NoSuchFieldException, NoSuchMethodException {

        // Auto-generated method stub

        Person person = new Person();
        path = person.getClass().getPackage().getName() + ".";
        System.out.println(person.getClass().getPackage().getName());
        //Demo1.  通过Java反射机制得到类的包名和类名
        Demo1();
        System.out.println("===============================================");
        //Demo2.  验证所有的类都是Class类的实例对象
        Demo2();
        System.out.println("===============================================");
        //Demo3.  通过Java反射机制，用Class 创建类对象[这也就是反射存在的意义所在]，无参构造
        Demo3();
        System.out.println("===============================================");
        //Demo4:  通过Java反射机制得到一个类的构造函数，并实现构造带参实例对象
        Demo4();
        System.out.println("===============================================");
        //Demo5:  通过Java反射机制操作成员变量, set 和 get
        Demo5();
        System.out.println("===============================================");
        //Demo6: 通过Java反射机制得到类的一些属性： 继承的接口，父类，函数信息，成员信息，类型等
        Demo6();
        System.out.println("===============================================");
        //Demo7: 通过Java反射机制调用类中方法
        Demo7();
        System.out.println("===============================================");
        //Demo8: 通过Java反射机制获得类加载器
        Demo8();
        System.out.println("===============================================");
    }
    /**
     * Demo1: 通过Java反射机制得到类的包名和类名
     */
    public static void Demo1()
    {
        Person person = new Person();
        System.out.println("Demo1: 包名: " + person.getClass().getPackage().getName() + "，"
                + "完整类名: " + person.getClass().getName());
    }
    /**
     * Demo2: 验证所有的类都是Class类的实例对象
     * @throws ClassNotFoundException
     */
    public static void Demo2() throws ClassNotFoundException
    {
        //定义两个类型都未知的Class , 设置初值为null, 看看如何给它们赋值成Person类
        Class<?> class1 = null;
        Class<?> class2 = null;
        //写法1, 可能抛出 ClassNotFoundException [多用这个写法]
        class1 = Class.forName(path + "Person");
        System.out.println("Demo2:(写法1) 包名: " + class1.getPackage().getName() + "，"
                + "完整类名: " + class1.getName());
        //写法2
        class2 = Person.class;
        System.out.println("Demo2:(写法2) 包名: " + class2.getPackage().getName() + "，"
                + "完整类名: " + class2.getName());
    }
    /**
     * Demo3: 通过Java反射机制，用Class 创建类对象[这也就是反射存在的意义所在]
     * @throws ClassNotFoundException
     * @throws IllegalAccessException
     * @throws InstantiationException
     */
    public static void Demo3() throws ClassNotFoundException, InstantiationException, IllegalAccessException
    {
        Class<?> class1 = null;
        class1 = Class.forName(path + "Person");
        //由于这里不能带参数，所以你要实例化的这个类Person，一定要有无参构造函数哈～
        Person person = (Person) class1.newInstance();
        person.setAge(20);
        person.setName("LeeFeng");
        System.out.println("Demo3: " + person.getName() + " : " + person.getAge());
    }
    /**
     * Demo4: 通过Java反射机制得到一个类的构造函数，并实现创建带参实例对象
     * @throws ClassNotFoundException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     * @throws InstantiationException
     * @throws IllegalArgumentException
     */

    public static void Demo4() throws ClassNotFoundException, IllegalArgumentException, InstantiationException, IllegalAccessException, InvocationTargetException

    {
        Class<?> class1 = null;
        Person person1 = null;
        Person person2 = null;
        class1 = Class.forName(path + "Person");
        //得到一系列构造函数集合
        Constructor<?>[] constructors = class1.getConstructors();
        person1 = (Person) constructors[0].newInstance();//构造函数列表完成构造，按照上下文顺序提取
        person1.setAge(30);
        person1.setName("leeFeng");
        person2 = (Person) constructors[1].newInstance(20,"leeFeng");
        System.out.println("Demo4: " + person1.getName() + " : " + person1.getAge()
                + "  ,   " + person2.getName() + " : " + person2.getAge()
        );
    }
    /**
     * Demo5: 通过Java反射机制操作成员变量, set 和 get
     *
     * @throws IllegalAccessException
     * @throws IllegalArgumentException
     * @throws NoSuchFieldException
     * @throws SecurityException
     * @throws InstantiationException
     * @throws ClassNotFoundException
     */

    public static void Demo5() throws IllegalArgumentException, IllegalAccessException, SecurityException, NoSuchFieldException, InstantiationException, ClassNotFoundException
    {
        Class<?> class1 = null;
        class1 = Class.forName(path + "Person");
        Object obj = class1.newInstance();
        Field personNameField = class1.getDeclaredField("name");
        personNameField.setAccessible(true);//权限问题很重要
        personNameField.set(obj, "胖虎先森");
        System.out.println("Demo5: 修改属性之后得到属性变量的值：" + personNameField.get(obj));
    }
    /**
     * Demo6: 通过Java反射机制得到类的一些属性： 继承的接口，父类，函数信息，成员信息，类型等
     * @throws ClassNotFoundException
     */
    public static void Demo6() throws ClassNotFoundException
    {
        Class<?> class1 = null;
        class1 = Class.forName(path + "SuperMan");
        //取得父类名称
        Class<?>  superClass = class1.getSuperclass();
        System.out.println("Demo6:  SuperMan类的父类名: " + superClass.getName());
        System.out.println("===============================================");
        Field[] fields = class1.getDeclaredFields();
        for (int i = 0; i < fields.length; i++) {
            System.out.println("类中的成员: " + fields[i]);
        }
        System.out.println("===============================================");
        //取得类方法
        Method[] methods = class1.getDeclaredMethods();
        for (int i = 0; i < methods.length; i++) {
            System.out.println("Demo6,取得SuperMan类的方法：");
            System.out.println("函数名：" + methods[i].getName());
            System.out.println("函数返回类型：" + methods[i].getReturnType());
            System.out.println("函数访问修饰符：" + Modifier.toString(methods[i].getModifiers()));
            System.out.println("函数代码写法： " + methods[i]);
        }
        System.out.println("===============================================");
        //取得类实现的接口,因为接口类也属于Class,所以得到接口中的方法也是一样的方法得到哈
        Class<?> interfaces[] = class1.getInterfaces();
        for (int i = 0; i < interfaces.length; i++) {
            System.out.println("实现的接口类名: " + interfaces[i].getName() );
        }
    }
    /**
     * Demo7: 通过Java反射机制调用类方法
     * @throws ClassNotFoundException
     * @throws NoSuchMethodException
     * @throws SecurityException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     * @throws IllegalArgumentException
     * @throws InstantiationException
     */

    public static void Demo7() throws ClassNotFoundException, SecurityException, NoSuchMethodException, IllegalArgumentException, IllegalAccessException, InvocationTargetException, InstantiationException
    {
        Class<?> class1 = null;
        class1 = Class.forName(path + "SuperMan");
        System.out.println("Demo7: \n调用无参方法fly()：");
        Method method = class1.getMethod("fly");
        method.invoke(class1.newInstance());
        System.out.println("调用有参方法walk(int m)：");
        method = class1.getMethod("walk",int.class);
        method.invoke(class1.newInstance(),100);
    }
    /**
     * Demo8: 通过Java反射机制得到类加载器信息
     *
     * 在java中有三种类类加载器。[这段资料网上截取]
     1）Bootstrap ClassLoader 此加载器采用c++编写，一般开发中很少见。
     2）Extension ClassLoader 用来进行扩展类的加载，一般对应的是jre\lib\ext目录中的类
     3）AppClassLoader 加载classpath指定的类，是最常用的加载器。同时也是java中默认的加载器。
     *
     * @throws ClassNotFoundException
     */

    public static void Demo8() throws ClassNotFoundException
    {
        Class<?> class1 = null;
        class1 = Class.forName(path + "SuperMan");
        String nameString = class1.getClassLoader().getClass().getName();
        System.out.println("Demo8: 类加载器类名: " + nameString);
    }
}
```

## 3.6. 参考
1. <a href = "https://www.jianshu.com/p/9be58ee20dee">Java高级特性——反射</a>
2. <a href = "https://www.cnblogs.com/mengdd/archive/2013/01/26/2877972.html">Java中的反射机制(一)</a>
3. <a href = "https://blog.csdn.net/ljphhj/article/details/12858767">一个例子让你了解Java反射机制</a>