<!-- TOC -->

- [1. 数据结构](#1-数据结构)
- [2. 数组](#2-数组)
  - [2.1. Array](#21-array)
  - [2.2. ArrayList<>](#22-arraylist)
  - [2.3. List](#23-list)
    - [2.3.1. 部分的方法](#231-部分的方法)
- [3. Map](#3-map)
  - [3.1. 常见的使用的操作](#31-常见的使用的操作)
  - [3.2. AbstractMap](#32-abstractmap)
    - [3.2.1. 单键值对类 SimpleEntry<?,?>](#321-单键值对类-simpleentry)
  - [3.3. 参考](#33-参考)

<!-- /TOC -->
# 1. 数据结构  
类型|特点
--|--
数组|创建数组是每个引用被初始化为null，这样子在运行是如果遇到null，java会报错。

# 2. 数组

## 2.1. Array
1. char数组在`char[] title = title1;`的情况下，引用的是相同数组，对其中一个改变会影响另一个
2. 如果要简单的拷贝
    1. for循环一个一个进行复制
    2. System.arraycopy()
        + public static native void arraycopy(Object src,  int  srcPos,Object dest, int destPos,int length);
    3. Arrays.copyOf()
    4. Object.clone()
3. Arrays.binarySearch(isbn,2)：二分查找关键字2的返回值(下标)
4. 声明一维数组：`String[] p / String a []`
5. Arrays.fill(array_name,char):用来将array_name的数组填充为char

## 2.2. ArrayList<>
1. 实例化`ArrayList<String> name = new ArrayList<?>`

## 2.3. List
1. List接口是继承Collection接口，所以也继承了Collection集合中有的方法。

### 2.3.1. 部分的方法
方法描述|方法功能
--|--
`void add(int index,E element)`|在指定位置插入元素，后面的元素往后移动一个元素
`boolean addAll(int index,Collection<? extends E> c)`|在指定的位置中插入c集合全部的元素，如果集合发生改变，则返回true，否则返回false
`E get(int index)`|返回list集合中指定索引位置的元素
`int indexOf(Object o)`|返回list集合中第一次出现o对象的索引位置，如果list集合中没有o对象，那么返回-1
`ListIterator<E>listIterator()`|返回此列表元素的列表迭代器(按适当顺序)
`ListIterator<E> listterator`|从指定位置开始，返回此列表元素的列表迭代器(按适当顺序)
`E remove(int index)`|删除指定索引的对象
`E set(int index,E element)`|在索引为index位置的元素更改为element
`List<E>subList(int fromIndex, int toIndex)`|返回从索引fromIndex到toIndex的元素集合，包左不包右

# 3. Map
1. Map集合是Key和Value
2. 实例化:`Map map = new HashMap();`

## 3.1. 常见的使用的操作
方法名|功能
--|--
`V put(K key,V value)`|向map集合中添加Key为key，Value为value的元素，当添加成功时返回null，否则返回value，key不可以重复
`void putAll(Map<? extends K,? extends V> m)`|向map集合中添加指定集合的所有元素
`void clear()`|把map集合中所有的键值删除
`boolean containsKey(Object key)`|检查是否有这个键值的元素
`boolean containsValue(Object value)`|检查map集合中有没有包含value的元素
`Set<Map.Entry<K,V>> entrySet()`|返回map到一个Set集合中
`boolean equals(Object o)`|判断两个Set集合的元素是否相同
`V get(Object key)`|根据map集合中元素的Key来获取相应元素的Value
`int hashCode()`|返回map集合的哈希码值
`boolean isEmpty()`|检出map集合中是否有元素
`Set<K> KeySet()`|返回map集合中所有key
`V remove(Object key)`|删除Key为key值的元素
`int size()`|返回map集合中元素个数
`Collection<V> values()`|返回map集合中所有的value到一个Collection集合

## 3.2. AbstractMap
1. 实现一些简单的通用的方法。
2. 抽象类通常作用一种骨架来完成公共的方法。
3. 提供了一个protected修饰的无参构造方法，意味着只有它的子类才能访问，也就会只有其子类才能调用这个无参的构造方法。

### 3.2.1. 单键值对类 SimpleEntry<?,?>
1. 实例化:`AbstractMap.SimpleEntry<?,?> name = new AbstracMap.SimpleEntry<>(param1,param2);`

部分操作|功能
--|--
`name.getKey()`|元组中的第一个元素
`name.getValue()`|元组中的第二个元素
`name.setValue()`|设置元组中的第二个元素

## 3.3. 参考
1. <a href = "https://www.cnblogs.com/xiaostudy/p/9510763.html">java中Map集合的常用方法</a>