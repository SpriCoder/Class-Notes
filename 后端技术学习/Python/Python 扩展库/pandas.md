<!-- TOC -->

- [1. Pandas](#1-pandas)
  - [1.1. DataFrame](#11-dataframe)
    - [1.1.1. 创建DataFrame](#111-创建dataframe)
    - [1.1.2. 查看相关属性](#112-查看相关属性)
    - [1.1.3. 基本操作](#113-基本操作)
  - [1.2. Series结构](#12-series结构)
    - [1.2.1. 创建的方式](#121-创建的方式)
    - [1.2.2. 基本运算](#122-基本运算)
    - [1.2.3. name属性](#123-name属性)
  - [1.3. 计算相关性关系](#13-计算相关性关系)
- [2. pandas处理大型的文件](#2-pandas处理大型的文件)
  - [2.1. 读取限定列](#21-读取限定列)
  - [2.2. 读取限定行](#22-读取限定行)
  - [2.3. 分块读取](#23-分块读取)
  - [2.4. 获取头和尾部](#24-获取头和尾部)
  - [2.5. 使用pandas分块处理大文件](#25-使用pandas分块处理大文件)
  - [2.6. 参考](#26-参考)

<!-- /TOC -->

# 1. Pandas
1. 基于SciPy和NumPy
2. 高效的Series和DataFrame数据结构
3. 强大的可扩展数据操作与分析的Python库
4. 高效处理大数据集的切片等功能
5. 提供优化库功能读写多种文件格式，如CSV、HDF5

## 1.1. DataFrame
1. 可以切片访问
2. 一个表格型的数据结构，包含一组类似于index的有序列，每一列的值可以使不同的
3. 使用`head(number)`来访问前几行
4. 使用`tail(number)`来访问后几行
5. 使用`loc[index]`来按照字段进行访问第几段数据

### 1.1.1. 创建DataFrame
1. 由列表创建
2. 由元组创建
3. 由字典创建
```python
data = {'name':['1','2','3'],'pay':[100,200,300]}
frame = pd.DataFrame(data)
# frame格式如下
#    name    pay
#0      1    100
#1      2    200
#2      3    300
```
4. 由ndarray创建
```python
data = np.array([('1',100),('2',200),('3',300)])
frame = pd.DataFrame(data,index = range(1,4),columns = ['name','pay'])
# frame格式如下
#    name    pay
#1      1    100
#2      2    200
#3      3    300
```
5. 由Series创建
6. 由文件创建

### 1.1.2. 查看相关属性
1. `.index`查看行索引
2. `.columns`查看列索引
3. `.values`查看值
4. 修改直接赋值:`frame.index = range(2,5)...`

### 1.1.3. 基本操作
1. 选择操作:`frame['name']/frame.name`
2. iloc(,):第一维表示行，第二维表示列
3. 对对象进行修改:直接赋值`frame['name']='admin'`
4. 删除某一列:`del frame['name']`
5. 一般查找先取列，再说行，然后结果会被转成字符串
6. 条件查找:`frame[expr(frame.pay>=5000)]`
    + 先判断再输出
7. 插入行:`frame.loc(number)={(,),(,),(,)}`
8. 插入列:`frame[name] = [,,]`
8. 删除行:`frame.drop(number)`
9. 修改列索引:`frame.columns = ['1',...]`
10. 调整index:`frame.index = range(0,len(frame) + 1)`
11. `frame.apply(func,axis = 0)`按照相应列每一项应用func进行处理后导入返回
12. `applymap()`方法
13. `map()`方法
14. 可以直接用列与列之间进行加减乘除默认对应项操作`frame['1'] = frame['2'] + frame['3']`

## 1.2. Series结构
1. 可边长的字典，相当于定长有序字典。
2. 由数据和索引组成，类似一维数组的对象。
3. 具有数据对齐功能

### 1.2.1. 创建的方式
1. `pd.Series(list1，index=list2)`
    + 结果默认自带0基索引
    + 如果指定索引使用index来指定

### 1.2.2. 基本运算
1. 乘法*:对每一项都做
2. 乘方:np.exp(aSer)
3. 加法:对应元素对应项相加
4. `pd.isnull(aSer)`检测是否是空的。

### 1.2.3. name属性
1. Series对象本身及其索引均有一个name属性
2. `aSer.name = str`比较像是字段名
3. `aSer.index.name = str`

## 1.3. 计算相关性关系
1. `D.corr()`可以来计算相应数据间的相关性关系。
    + 并且提取出截距、斜率等拟合数据

# 2. pandas处理大型的文件
1. 在训练机器学习模型的过程中，源数据常常不符合我们的要求，所以我们需要对数据进行清洗。

## 2.1. 读取限定列
1. 我们可以在读取csv文件的过程中，我们可以只提取限定列
```py
file = pd.read_csv('demo.csv',usecols = ['column1','column2'])
```

## 2.2. 读取限定行
```py
file = pd.read_csv('demo.csv',nrows=1000,usecols=['column1', 'column2', 'column3'])
```

## 2.3. 分块读取
```py
reader = pd.read_csv('demo.csv',nrows=10000,
                     usecols=['column1','column2','column3'], 
                     chunksize=1000,iterator=True)
reader
```

## 2.4. 获取头和尾部
```py
file = pd.read_csv('demo.csv')
df = pd.DataFrame(file)
df.head() # 默认前10条数据
df.tail() # 默认后10条数据
```

## 2.5. 使用pandas分块处理大文件
1. 读取文件的参数中有两个参数:chunksize,iterator
2. 原理是不一次性把文件数据读入内存，而是分多次

## 2.6. 参考
1. <a href = "https://blog.csdn.net/wld914674505/article/details/81431128">亿级数据清洗</a>