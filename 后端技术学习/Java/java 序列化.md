<!-- TOC -->

- [1. 存储对象](#1-存储对象)
  - [1.1. 序列化](#11-序列化)
    - [1.1.1. 序列化对象写入文件](#111-序列化对象写入文件)
    - [1.1.2. 会被序列化的东西](#112-会被序列化的东西)
    - [1.1.3. transient关键字](#113-transient关键字)
  - [1.2. 解序列化](#12-解序列化)
    - [1.2.1. 解序列化过程](#121-解序列化过程)
    - [1.2.2. 详解解序列化过程](#122-详解解序列化过程)
  - [1.3. 写纯文本文件](#13-写纯文本文件)
- [2. JSON和Java的使用](#2-json和java的使用)
  - [2.1. JSON格式的编码(生成JSON对象)](#21-json格式的编码生成json对象)
  - [2.2. JSON格式的解码](#22-json格式的解码)
  - [2.3. JSON对象和字符串的相互转化](#23-json对象和字符串的相互转化)
  - [2.4. 参考](#24-参考)

<!-- /TOC -->
**序列化以及文件输入输出**
1. 对象有状态和行为，行为存在于类中，而状态存在于个别的对象中，方便被非java程序读取

# 1. 存储对象

## 1.1. 序列化
1. 只有自己写的java程序会用到这些数据
2. 更加的安全
3. 如果让类可以序列化，实现Serializable接口
4. 可以父类不被序列化，但是子类实现序列化
5. 序列化过程中会辨别两个对象是否相同，这种情况下只有一个对象会被保存，剩下的会被指向这个引用

### 1.1.1. 序列化对象写入文件
1. 创建文件输出流
    + `FileOutputStream fileStream = new FileOutputStream("MyGame.ser")`
    + 文件不存在会直接创建出来
2. 创建对象输出流
    + `ObjectOutputStream os = new ObjectOutputStream(fileStream);`
3. 写入对象:
    + `os.writeObject(character);`
4. 关闭流：
    + `os.close();`
5. 整个过程就是对象被碾平，然后当做字节写入文件

### 1.1.2. 会被序列化的东西
1. 所有的实例变量
2. 所有被引用的对象
3. 保证里面的都可以被序列化

### 1.1.3. transient关键字
1. 用于某个实例变量不应该或不能被序列化

## 1.2. 解序列化
1. 静态变量还原的时候，会维持原来的样子，而不是存储的样子
    + 也就是说静态变量是归属于类所有的，不会被序列化，在解序列化的时候维持类声明时的状态
### 1.2.1. 解序列化过程
1. 创建文件读入流
    + `FileInputStream filestream = new FileInputStream("name");`
2. 创建对象读入流
    + `ObjectInputStream os = new ObjectInputStream(fileStream);`
3. 读入对象
    + `Object a = os.readObject();`
4. 转换对象类型
    + `Game g = (Game) a;`
5. 关闭对象读入流
    + `os.close();`

### 1.2.2. 详解解序列化过程
1. 从流中读出对象
2. java虚拟机通过存储信息判断class类型
3. java虚拟机尝试寻找和加载对象的类。如果找不到会抛出异常
4. 新的对象会被放置到堆上，但是构造函数不会执行
5. 如果对象在继承书上有一个不可以序列化的祖先类，那么该不可序列化类以及在它之上的类的构造函数会被执行
6. 对象的实例变量全部恢复，transient的变量会被赋值为null的对象引用或者primitive的主类型默认值

## 1.3. 写纯文本文件
1. 会被其他程序使用到相应的数据
    + 比如写成用Tab字符来分隔的档案，方便数据库识别
2. 使用FileWriter来代替FileOutputStream
    1. 使用try...catch
    2. FileWriter writer = new FileWriter(src)
    3. 最后记着关闭.close()
3. 使用BufferedWriter写纯文本
    1. 强制写入：`.flush()`

# 2. JSON和Java的使用
1. 环境配置:(maven方式)

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.47</version>
</dependency>
```

## 2.1. JSON格式的编码(生成JSON对象)
```java
public void testJson() {
    JSONObject object = new JSONObject();
    //string
    object.put("string","string");
    //int
    object.put("int",2);
    //boolean
    object.put("boolean",true);
    //array
    List<Integer> integers = Arrays.asList(1,2,3);
    object.put("list",integers);
    //null
    object.put("null",null);
​
    System.out.println(object);
    //{"boolean":true,"string":"string","list":[1,2,3],"int":2}
}
```

## 2.2. JSON格式的解码
```java
public void testJson2() {
​
  JSONObject object = JSONObject
      .parseObject("{\"boolean\":true,\"string\":\"string\",\"list\":[1,2,3],\"int\":2}");
  //string
  String s = object.getString("string");
  System.out.println(s);
  //int
  int i = object.getIntValue("int");
  System.out.println(i);
  //boolean
  boolean b = object.getBooleanValue("boolean");
  System.out.println(b);
  //list
  List<Integer> integers = JSON.parseArray(object.getJSONArray("list").toJSONString(),Integer.class);
  integers.forEach(System.out::println);
  //null 一种高级的输出方法
  System.out.println(object.getString("null"));
​
}
```

## 2.3. JSON对象和字符串的相互转化
方法|作用
--|--
JSON.parseObject()|从字符串解析 JSON 对象
JSON.parseArray()|从字符串解析 JSON 数组
JSON.toJSONString(obj/array)|将 JSON 对象或 JSON 数组转化为字符串

```java
//从字符串解析JSON对象
JSONObject obj = JSON.parseObject("{\"runoob\":\"菜鸟教程\"}");
//从字符串解析JSON数组
JSONArray arr = JSON.parseArray("[\"菜鸟教程\",\"RUNOOB\"]\n");
//将JSON对象转化为字符串
String objStr = JSON.toJSONString(obj);
//将JSON数组转化为字符串
String arrStr = JSON.toJSONString(arr);
```

## 2.4. 参考
<a href = "https://www.runoob.com/w3cnote/java-json-instro.html">JSON+Java</a>