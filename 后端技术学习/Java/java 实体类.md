<!-- TOC -->

- [1. 字符串相关类](#1-字符串相关类)
  - [1.1. String类的常用方法](#11-string类的常用方法)
    - [1.1.1. StringBuffer和StringBuilder](#111-stringbuffer和stringbuilder)
    - [1.1.2. 部分常用的string方法](#112-部分常用的string方法)
  - [1.2. java字符串的格式化](#12-java字符串的格式化)
  - [1.3. StringTokenizer](#13-stringtokenizer)
    - [1.3.1. 构造方法](#131-构造方法)
    - [1.3.2. 常用方法](#132-常用方法)
    - [1.3.3. 代码实现](#133-代码实现)
- [2. Math类](#2-math类)
  - [2.1. 方法:](#21-方法)
- [3. File类](#3-file类)
- [4. 枚举类 Enum](#4-枚举类-enum)
  - [4.1. 枚举类的常见应用](#41-枚举类的常见应用)
  - [4.2. 参考](#42-参考)
- [5. Applet类](#5-applet类)
- [6. Collections](#6-collections)

<!-- /TOC -->

# 1. 字符串相关类
1. 字符串是不可变类，本质上是按照字符数组存储的
```java
String a = "hello";
String b = "hello";
String c = new String("hi");
String d = new String("hi");
char[] e = {'h','i'};
a==b;//true
c==d;//false
a==e;//编译错误
a.equals(e);//true，务必注意这件事情
```
## 1.1. String类的常用方法
1. String类对象保存不可修改的Unicode字符序列
2. String类的下述方法能创建并返回一个新的String对象
3. 功能性方法：endsWith、startsWith、indexOf、lastIndexOf、equals、equalsIgnoreCase、compareTo
4. 其他方法:charAt、length、replace()

### 1.1.1. StringBuffer和StringBuilder
1. StringBuffer类：
    1. 构造方法：
        1. `StringBuffer()`:创建一个字符序列为空的StringBuffer对象
            + `StringBuffer(int capacity)`:用来创建指定大小的StringBuffer
        2. `StringBuffer(String str)`:创建一个把与String对象str相同的字符序列
    2. 其他方法：
        1. append()方法：想StringBuffer对象添加字符序列
        2. delete(int start,int end):删除从start开始到end-1为止的一端字符序列，返回对象引用
        3. deleteCharAt(int index):删除位于序列指定位置上的char
        4. insert(int offset,String str):可以在指定位置上插入字符序列等
        5. insert(int offest,double d):插入一个浮点数，提供自动转化
        6. setCharAt(int index，char ch):在指定位置上插入指定字符
        7. reverse():将当前的字符串反转
2. 区分String和StringBuffer和StringBuilder：
    1. String：不可变字符序列
    2. StringBuffer：可变字符序列，并且线程安全，但是效率低
    3. StringBuilder：可变字符序列，线程不安全，但是效率高

### 1.1.2. 部分常用的string方法

方法名|功能|其他备注
--|--|--
toCharArray()|将string处理成为char数组|-
substring()|取其子字符串|允许可以提供index
concat(str)|连接两个字符串|(可以进一步区别与+的区别)
startsWith(str)|判断一个字符串是不是以这个开头|-

## 1.2. java字符串的格式化
```java
//保留两位小数
public static void main(String[] args) {
    double d = 756.2345566;
    //方法一：最简便的方法，调用DecimalFormat类
    DecimalFormat df = new DecimalFormat(".00");
    System.out.println(df.format(d));
    //方法二：直接通过String类的format函数实现
    System.out.println(String.format("%.2f", d));
    //方法三：通过BigDecimal类实现
    BigDecimal bg = new BigDecimal(d);
    double d3 = bg.setScale(2, BigDecimal.ROUND_HALF_UP).doubleValue();
    System.out.println(d3);
    //方法四：通过NumberFormat类实现
    NumberFormat nf = NumberFormat.getNumberInstance();
    nf.setMaximumFractionDigits(2);
    System.out.println(nf.format(d));
}
```

## 1.3. StringTokenizer
1. 属于`java.util`包

### 1.3.1. 构造方法
1. `StringTokenizer(String str)`:构造一个用来解析 str 的 StringTokenizer 对象。
   - java 默认的分隔符是空格("")、制表符(\t)、换行符(\n)、回车符(\r)。
3. `StringTokenizer(String str, String delim)`:构造一个用来解析 str 的 StringTokenizer 对象，并提供一个指定的分隔符。
4. `StringTokenizer(String str, String delim, boolean returnDelims)`:构造一个用来解析 str 的 StringTokenizer 对象，并提供一个指定的分隔符，同时，指定是否返回分隔符。

### 1.3.2. 常用方法
1. `int countTokens()`:返回nextToken方法被调用的次数。
2. `boolean hasMoreTokens()`:返回是否还有分隔符。
3. `boolean hasMoreElements()`:判断枚举 （Enumeration） 对象中是否还有数据。
4. `String nextToken()`:返回从当前位置到下一个分隔符的字符串。
5. `Object nextElement()`:返回枚举 （Enumeration） 对象的下一个元素。
6. `String nextToken(String delim)`:与 4 类似，以指定的分隔符返回结果。

### 1.3.3. 代码实现
```java
import java.util.*;
public class Main { 
    public static void main(String[] args){ 
        String str = "runoob,google,taobao,facebook,zhihu";
        // 以 , 号为分隔符来分隔字符串
        StringTokenizer st=new StringTokenizer(str,",");
        while(st.hasMoreTokens()) { 
            System.out.println(st.nextToken());
        }
    }
}
```

# 2. Math类
1. 来源:java.lang.Math

## 2.1. 方法:
+ abs:绝对值
+ acos,asin,atan,cos,sin,tan：各种三角函数
+ sqrt：平方根
+ pow(double a,double b)：a的b次幂
+ log自然对数
+ exp e为底指数
+ max(double a,double b)
+ min(double a,double b)
+ random()：返回0.0到1.0的随机数
+ long round(double a)：double类型的数据a转化为long类型
+ toDegrees(double angrad)：弧度->角度
+ toRadians(double angdeg)：角度->弧度


# 3. File类
1. 来源：java.io.File类
2. 常见的构造方法：`public File(String pathname)`
    + 构造方法参数可以为String、File、URL
3. 方法：
    + public boolean canRead()
    + public boolean canWrite()
    + public boolean exists()     
    + public boolean isDirectory()
    + public boolean isFile()     
    + public boolean isHidden()
    + public long lastModified() 
    + public long length()
    + public String getName()   
    + public String getPath()
    + public String getAbsolutePath()
    + public boolean delete()
    + public boolean mkdir():创建新的文件夹
4. equals()方法在这个类中被重写，只有两个对象的内容和类型一致时返回

# 4. 枚举类 Enum
1. 只能够取特定值中的一个
2. 使用enum关键字
3. 所有的枚举类型都隐形地继承自java.lang.Enum。而每个被枚举的变量都被默认为public static final的
4. 建议使用一组常量的时候，使用枚举类型
5. 尽量避免使用枚举的高级特性

## 4.1. 枚举类的常见应用
1. 常量定义
```java
public enum WeekDay {
    SUN, MON, TUE, WED, THT, FRI, SAT
}
```
2. 结合switch
```java
public enum WeekDay {
    SUN, MON, TUE, WED, THT, FRI, SAT
}
public class SelectDay{
    WeekDay weekday = WeekDay.SUN;
    public void select(){
        switch(weekday){
            case SUN:
                weekday = WeekDay.SUN;
                bread;
            ...
        }
    }
}
```
3. 向枚举添加新方法
```java

public enum Color {  
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);  
    // 成员变量  
    private String name;  
    private int index;  
    // 构造方法  
    private Color(String name, int index) {  
        this.name = name;  
        this.index = index;  
    }
    // 普通方法  
    public static String getName(int index) {
        for (Color c : Color.values()) {  
            if (c.getIndex() == index) {  
                return c.name;  
            }
        }  
        return null;  
    }  
    // get set 方法  
    public String getName() {  
        return name;  
    }
    public void setName(String name) {  
        this.name = name;  
    }  
    public int getIndex() {  
        return index;  
    }  
    public void setIndex(int index) {  
        this.index = index;  
    }  
}  
```
4. 覆盖枚举方法
```java
public enum Color {
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4);
    // 成员变量
    private String name;
    private int index;
    // 构造方法
    private Color(String name, int index) { 
        this.name = name;
        this.index = index;
    } 
    //覆盖方法
    @Override
    public String toString() {
        return this.index+"_"+this.name;
    }
}
```
5. 实现接口
```java
public interface Behaviour { 
    void print(); 
    String getInfo(); 
} 
public enum Color implements Behaviour{ 
    RED("红色", 1), GREEN("绿色", 2), BLANK("白色", 3), YELLO("黄色", 4); 
    // 成员变量
    private String name; 
    private int index; 
    // 构造方法 
    private Color(String name, int index) { 
        this.name = name; this.index = index; 
    } 
    //接口方法 
    @Override 
    public String getInfo() { 
        return this.name; 
    } 
    //接口方法 
    @Override 
    public void print() { 
        System.out.println(this.index+":"+this.name); 
    } 
}
```
6. 枚举组织集合
```java
public interface Food {
    enum Coffee implements Food{
        BLACK_COFFEE,DECAF_COFFEE,LATTE,CAPPUCCINO
    }
    enum Dessert implements Food{
        FRUIT, CAKE, GELATO
    }
}
```
7. 枚举集合
```java
public class Test {
    public static void main(String[] args) {
        EnumSet<WeekDay> week = EnumSet.noneOf(WeekDay.class);
        week.add(WeekDay.MON);
        System.out.println("EnumSet中的元素：" + week);
        week.remove(WeekDay.MON);
        System.out.println("EnumSet中的元素：" + week);
        week.addAll(EnumSet.complementOf(week));
        System.out.println("EnumSet中的元素：" + week);
        week.removeAll(EnumSet.range(WeekDay.FRI, WeekDay.SAT));
        System.out.println("EnumSet中的元素：" + week);
    }
}
```

## 4.2. 参考
1. <a href = "https://blog.csdn.net/echizao1839/article/details/80890490">Java enum常见的用法</a>

# 5. Applet类
1. 是一个采用java编程语言编写的小应用程序
2. 可以读取客户端部分系统变量
3. 不可以读取客户端文件
4. 不可以在客户端主机上创建新文件
5. 不可以在客户端装载程序库
6. `<applet code=TestApplet.class width=100 height=100></applet>`

# 6. Collections
```java
Collections.sort( occupiedSegs, new Comparator<AbstractMap.SimpleEntry<Integer, Integer>>() {  
// 按起始位置从小到大排序
            @Override
            public int compare(AbstractMap.SimpleEntry<Integer, Integer> p1, AbstractMap.SimpleEntry<Integer, Integer> p2) {
                return p1.getKey()-p2.getKey();
            }
        } );
```