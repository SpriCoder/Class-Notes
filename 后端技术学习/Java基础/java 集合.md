<!-- TOC -->

- [1. 哈希（类似于 python dict)](#1-哈希类似于-python-dict)
- [2. List类：](#2-list类)
  - [2.1. 常见的list的初始化方式](#21-常见的list的初始化方式)
  - [2.2. 集合关系运算](#22-集合关系运算)
- [3. Map类：](#3-map类)
- [4. ArrayList类](#4-arraylist类)
- [5. Vector类](#5-vector类)
- [6. java集合进行排序的方式](#6-java集合进行排序的方式)
  - [6.1. 第一种排序方式](#61-第一种排序方式)
  - [6.2. 第二种排序方式](#62-第二种排序方式)
  - [6.3. 实例](#63-实例)
  - [6.4. 参考](#64-参考)

<!-- /TOC -->
**集合**

# 1. 哈希（类似于 python dict)  
1. HashMap 是一个用于储存Key-Value键对的集合。每一个键值对叫Entry,分散存储在一个数组中，作为一个主干，初始值null
 
# 2. List类：  

## 2.1. 常见的list的初始化方式
1. 构造List后使用List.add初始化
```java
List<String> stringList = new LinkedList<>();
stringList.add("a");
```
2. 使用{{}}双括号语法
   + 相当于定义了匿名内部类，会损失效率
   + 如果返回List到其他地方可能会引起内存泄露
```java
List<String> stringList = new LinkedList<String>(){{
   add("a");
   add("b");
}}
```
3. 使用Arrays.asList的内部静态方法
   1. 返回的是Arrays的静态内部类
      + 静态类继承自AbstractList，实现RandomAccess，内部使用数组来存储元素，但是不支持增删元素
   2. 尽量避免使用int基本类型，和[]这种底层的数据结构，相应的应当尽量使用List和Integer
```java
List<String> stringList  = Arrays.asList("a","b","c");
```

4. 使用高阶函数Stream(JDK8)
   1. 略微有点大材小用
```java
List<String> List = Stream.of("a","b").collect(Collectors.toList())
```

5. 使用Lists(JDK9)
```java
List<String> list = Lists.newArrayList("a","b","c");
```

## 2.2. 集合关系运算
1. 求并集:`list1.addAll(list2)`
2. 求交集:`list1.retainAll(list2)`
3. 求补集:`list1.removeAll(list2)`

# 3. Map类：
1. 用于存储元素对(称作"键"和"对")。
2. 类型介绍：
   1. 通用MAP
   2. 专用MAP
   3. 用于帮助我们实现自己的MAP类的抽象类
3. 类型区别
   1. HashMap:*最常用*  
   >用HashCpde值存储数据，访问数据，非同步的 
   2. TreeMap:  
   >能够把它保存的记录根据键值进行升序排序，也可以制定排序的比较器。  
   >不允许Key的值为null.不同步.  
   3. Hashtable：  
   >Key和value均不允许为null，它支持线程的同步，写入时较慢。  
   4. LinkedHashMap:  
   >保存了记录的插入顺序，用Iterator遍历时，先得到的记录是先插入的。  
   >Key和value不可以为空，非同步的。  
4. 常用语法  

代码|作用
--|--
`Map<String, String> map = new HashMap<String, String>();`|用于初识化一个链表
`map.put("key1","value1");"`|用于插入元素
`map.get("key1");`|用于获取元素
`map.remove("key1");`|用于移除元素
`map.clear`|用于清空整个链表
`map.Keyset()`|用于增强for进行增强遍历(例子参见1)
`map.entrySet()`|用于增强for进行另一种增强遍历(例子参见2)
`map.putAll(Map t)`|将指定的Map中的所有映射复制到这个map
`map.containsKey(Object key)`|如果Map包含指定键的映射，返回true
`map.containsValue(Object value)`|如果map将一个或多个键映射到指定值，则返回true
`map.isEmpty()`|如果Map不包含键-值映射，则返回true
`map.size()`|返回Map中的键-值映射的数目

   1. ```java
      for (String key : map.keySet()) {
      System.out.println(key + " ：" + map.get(key));
      }
      ```
   2. ```java
      for (Map.Entry<String, String> entry : map.entrySet()) {
      System.out.println(entry.getKey() + " ：" + entry.getValue());
      }
      ```

5. 迭代可以直接进行迭代，也可以用迭代器进行迭代。  

# 4. ArrayList类
1. 是数组队列，相当于动态数组，不同于Vector的是，这个类并不是线程安全的，也就是最好只在单线程中使用。
2. 构造函数  

方法名|作用
-----|---
`ArrayList`|默认构造函数
`ArrayList(int capacity)`|设置这个数组的默认大小，如果不够的话一次添加上一次容量的一半
`ArrayList(Collection<? extends E> collection)`|创建一个包含collection的ArrayList
3. API(常用)

返回值|方法|作用
--|--|--
boolean|add(E object)|添加项
boolean|addAll(collection<? extends E>collection)|移除全部项
void|clear()|清空
boolean|contains(Object object)|判断是否包含
boolean|containsAll(collection<? extends E>collection)|判断是否全部包含
boolean|equals(Object object)|判断是否相同
int|hashCode()|根据对象的地址或者字符串或者数字算出来的int型的数值
boolean|isEmpty()|判断是否为空
boolean|remove(Object object)|移除元素
boolean|removeAll(collection<? extends E>collection)|移除全部这些元素
boolean|retainAll(collection<? extends E>collection)|取交集
int|size()|返回这个链表的长度
|get()|按照索引访问元素
<a href = "https://www.cnblogs.com/skywang12345/p/3308556.html
">查询详情</a>

# 5. Vector类
1. 实现自动增长的对象数组
2. <a href = "https://www.cnblogs.com/zheting/p/7708366.html">详见</a>

# 6. java集合进行排序的方式

## 6.1. 第一种排序方式
1. Collections.sort(List list):第一种成为自然排序，参加排序的对象需要实现comparable接口

## 6.2. 第二种排序方式
1. Collections.sort(List list,Comparator c)

## 6.3. 实例
1. Person进行处理

```java
public class Person implements Comparable {

    private String name;
    private int age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public Person() {
        super();
    }
    public Person(String name, int age) {
        super();
        this.name = name;
        this.age = age;
    }
    @Override
    public String toString() {
        return "Emp [name=" + name + ", age=" + age + "]";
    }
    @Override
    public int compareTo(Object o) {
        if(o instanceof Person){
            Person emp = (Person) o;
            return this.name.compareTo(emp.getName());//换姓名升序排序
        }
        throw new ClassCastException("不能转换为Person类型的对象...");
    }

}

public class mainDemo {
   public static void main(String [] args){
         List<Person> list = new ArrayList<Person>();
         Person person1 = new Person("345",10);
         Person person2 = new Person("234",20);
         Person person3 = new Person("123",30);
         list.add(person2);
         list.add(person3);
         list.add(person1);

         for(Person person : list){
               System.out.println(person.toString());
         }

         //按照第一种情况进行排序
         Collections.sort(list);
         
         //按照第二种情况进行排序
         Collections.sort(list,new Comparator() {
               @Override
               public int compare(Object o1, Object o2) {
                  if(o1 instanceof Person && o2 instanceof Person){
                     Person e1 = (Person) o1;
                     Person e2 = (Person) o2;
                     return e1.getAge() - e2.getAge();
                  }
                  throw new ClassCastException("不能转换为Emp类型");
               }
         });
         System.out.println("使用Comparator比较器升序排序后:");
         for(Object o : list){
               System.out.println(o);
         }
   }
}
```

## 6.4. 参考
<a href = "https://www.cnblogs.com/huangjinyong/p/9037588.html">java集合进行排序的两种方式</a>