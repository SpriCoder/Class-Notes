<!-- TOC -->

- [1. 写在前面](#1-写在前面)
- [2. 第一部分](#2-第一部分)
  - [2.1. 第一章 对象导论](#21-第一章-对象导论)
- [3. 第二章 一切都是对象](#3-第二章-一切都是对象)
  - [3.1. 引用](#31-引用)
  - [3.2. 数据类型](#32-数据类型)
    - [3.2.1. 主类型的部分基本类型的方法](#321-主类型的部分基本类型的方法)
    - [3.2.2. String...的用法](#322-string的用法)
  - [3.3. 程序逻辑](#33-程序逻辑)
    - [3.3.1. if语句](#331-if语句)
    - [3.3.2. for循环语句](#332-for循环语句)
  - [3.4. 数据存储](#34-数据存储)
  - [3.5. 输入输出](#35-输入输出)
- [4. 类](#4-类)
- [5. 注释](#5-注释)
- [6. 方法](#6-方法)
- [7. 内存和虚拟机](#7-内存和虚拟机)
- [8. 集合](#8-集合)
- [9. 面向对象编程(职责&协作)](#9-面向对象编程职责协作)
- [10. 软件工程建模](#10-软件工程建模)
- [11. 垃圾回收机制](#11-垃圾回收机制)
- [12. main函数的含义](#12-main函数的含义)
- [13. 运算符](#13-运算符)

<!-- /TOC -->

# 1. 写在前面  
>2019年4月9日，这个乌拉的学习笔记诞生了。
1. 程序，是一段静态的代码，它是应用软件执行的蓝本。

# 2. 第一部分  
1. java文件开头顺序
    1. package
    2. import
    3. class：可以不声明public

## 2.1. 第一章 对象导论  

>>对于过程进行抽象<br>
>>每个对象都有一个接口<br>
>>创建一个类，之后可以操作和创建类的任意个对象<br>

**程序开发人员分类**  
>类创建者：     构建类（隐藏类之中脆弱的部分）<br>
>客户端程序员： 收集各种用来实现应用开发的类<br>

**内部界定边界的关键字**  

关键字|解释
---|:---
public|     对于任何人可见
private|    对于除了类型创建者和类型的内部方法之外任何人不可访问
protected|  基本和private相同，仅仅是在继承是可以访问父类而已

**包访问权限**  
>它是默认的访问权限，常见于多个包之间的情况，指的是在这种权限下类访问一个包（库组件）中的成员，但是包外相当于private<br>

**复用具体实现**  
>组合：使用现有的类合成新的类<br>
>>1.通常被视为具有"has-a"关系<br><br>
>>2.如果动态发生，被通常称为聚合<br><br>
>>3.具有极大的灵活性，在不影响客户端代码的情况下，修改这些成员，当然同样可以在运行时修改这些成员（**而继承没有这种情况**）<br>

**继承**   
1.父类的变动会影响他的子类  
2.继承主要是寻找相似程度和相近的方法  
3.在子类中产生差异的方法有两种:  
>第一种：在子类中添加新的方法  
>第二种：在子类中覆盖父类中的方法（重载要求函数名、返回值、参数值类型均相同） 

4.继承关系“is-a”  
5.在很多的子类和父类之间我们应用替代原则来制作接口  

**多态**  
1.对于一类的物体我们使用泛化类型。  
2.输入的时候泛化类型给我们便利，但是在输出的时候泛化类型却很难确定到底执行哪一部分。  
3.在非面向对象程序设计的语言中，*前期绑定*被普遍使用。  
4.在面向对象程序设计的语言中，*后期绑定*被采用，也就意味着向对象发送消时，被调用的代码直到**运行**的时候才被确定。  
`dosomething(circle)`  
这个表示一个圆被传入之后进行操作。  
5.上述的这个可以被描述成为*向上转型*  

**单根继承**  
这是一个便利于GC的操作  

**容器**  

**参数化类型**  
1.具有泛型  

**对象的创建和声明期**  
1.将自动变量和限域变量放置在堆栈或者静态存储单元中  
2.在堆中动态的创建对象  

**异常处理**  
1.java中强制设置的、唯一可接受的错误报告方式。  

**并发编程**
1.同时的多线程编程，问题是会同时共享资源。  

**java和internet**  
***web是什么***  
>1.客户/服务器计算技术  
>2.客户端编程  
>3.服务端编程


# 3. 第二章 一切都是对象

## 3.1. 引用
`String s="baozi"`这是一个引用，并不是一个对象

## 3.2. 数据类型

基本类型|大小|说明|默认初识值
--|--|--|--
boolen|-|只能够取true和false|false
char|16b|-|'\u0000'(null)
byte|8b|-|(byte)0
short|16b|-|(short)0
int|32b|-2<sup>31</sup>-2<sup>31</sup>-1|0
long|64b|-|0L
float|32b|-|0.0f
double|64b|-|0.0d
void|-|-|-
BigInterger|-|准确的表示任何大小的整数值，不会对视任何的信息
BigDecimal|-|支持任何精度的定点数

1. 字节型和短整型只能起到限制数据的作用，并不能节省内存
    + 计算前会被提升到整形
    + byte a = byte + 1:编译错误，因为上面
    + 应该是= (byte)(byte + 1)
详见p55 java编程思想
2. double类型判断相同：不能用==，不然永远是不等于
    + 可以转化成字符串比是否相同
    + 可以用Double.doubleToLongBits(number)转换成long来比较

### 3.2.1. 主类型的部分基本类型的方法
1. `Integer.valueOf(Str,radix(按照几进制进行转换))`

### 3.2.2. String...的用法
1. String...是java5之后添加的功能，表示的是一个可边长的参数列表。
2. 语法是类型后跟...，表示此处接受的参数为0到多个Object类型的对象。
```java
/**
 * 校验json数据是否为空
 * @param param
 * @param paramKeyArray
 * @return
 */
public static WebResult checkParam(JSONObject param, String... paramKeyArray){
    WebResult webResult = new WebResult();
    webResult.ok();
    if(param != null) {
        List<String> nullParamList = new ArrayList<>();
        String paramKey = "";
        for(int i = 0; i < paramKeyArray.length; i ++) {
            paramKey = paramKeyArray[i];
            System.out.println(param.getString(paramKey));
            if(StringUtils.isEmpty(param.getString(paramKey))){
                nullParamList.add(paramKey);
            }
        }
        if(! nullParamList.isEmpty()) {
            StringBuffer sb = new StringBuffer();
            sb.append("参数[");
            int i = 0;
            for(String s : nullParamList) {
                if(i != 0) {
                    sb.append(",");
                }
                sb.append(s);
                i ++;
            }
            sb.append("]不能为空");
            webResult= new WebResult();
            webResult.fail(sb.toString());
        }
    }
    return webResult;
}
public static void main(String[] args) {
    String json = "{\"STAFF_ID\":\"\",\"LOGIN_TYPE\":\"\"}";
    JSONObject param = JSONObject.parseObject(json);
    WebResult webResult = checkParam(param, "STAFF_ID", "LOGIN_TYPE");
    System.out.println(webResult.getMessage());
}
```
3. <a href = "https://blog.csdn.net/hzr0523/article/details/86064417">String...的用法</a>

## 3.3. 程序逻辑

### 3.3.1. if语句
1. if()这个语句中括号内部必须是布尔类型的值。

### 3.3.2. for循环语句
1. 传统的for循环:`for(int i = 0;i < 100 ;i+++)`
2. 增强的for循环:`for(String number:sets)`

## 3.4. 数据存储
存储区|特点
--|--
寄存器|最快、数量有限、按需分配
堆栈|次快、指针向下分配内存、指针向上释放内存
堆|不关心寄存的时间长度，用于存放所有的java代码
常量储存|通常放置在程序内部，在嵌入式系统中存放在ROM中
非RAM存储|将对

## 3.5. 输入输出
语句|语句作用
--|--
`Syetem.out.print()`|标准打印但是不换行
`System.out.println()`|标准打印并且换行
`readTheFile()`|读入文件

# 4. 类

# 5. 注释
1. //之后的文字到行末均为注释。 

<a href="file:///D:/南京大学/大一第二学期/软件工程与计算1-2019/参考资料/Java编程思想(第4版).pdf">上次读到了第62页。</a>

# 6. 方法

# 7. 内存和虚拟机

# 8. 集合

# 9. 面向对象编程(职责&协作)

# 10. 软件工程建模

# 11. 垃圾回收机制

1. java的垃圾回收器要负责完成3件任务：
   + 分配内存
   + 确保被引用对象的内存不被错误回收
   + 回收不再被引用的对象的内存空间
2. 一般情况下，垃圾回收器在进行回收操作时，整个应用是被暂时中止的
3. 最基本的做法：分代回收
   + 内存区域被划分为不同世代，对象依据其存活的时间被保存在对应世代的区域中
   + 一般划分为三个世代:年轻、年老和永久
4. 年轻世代的内存区域进一步划分成：
   + 伊甸园:内存分配地方，一块连续的空闲内存区域
   + 两个存活区:始终有一个保持是一个空白的，删除后交换
5. 年老和永久世代的内存区域的算法:"标记-清除-压缩"

# 12. main函数的含义
1. 不同语言都可以写带参数的main函数，那么这是为什么呢？
2. C++的:`int main(int agrc,char**argv)`
    + 直接运行，xx.exe，在命令行中，exe后面是可以加参数的。
3. java的:`public static void main(String[] args)`
    + java是在某一个class中的。在java.class后面是可以加参数的
    + 通过args拿到命令行后续的参数数据

# 13. 运算符
1. ^:异或 xor
2. &:并且 and
3. |:或者 or
4. 这个是二元运算符，比如`'0'|'1'|'0' = '1'`