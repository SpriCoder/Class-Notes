<!-- TOC -->

- [1. 文件的打开，读写和关闭](#1-文件的打开读写和关闭)
  - [1.1. 文件的打开](#11-文件的打开)
  - [1.2. 标准文件](#12-标准文件)
- [2. 网络数据获取](#2-网络数据获取)
  - [2.1. 抓取](#21-抓取)
    - [2.1.1. 抓取编码出现问题的解决方案](#211-抓取编码出现问题的解决方案)
  - [2.2. 解析](#22-解析)
  - [BeatifulSoup中获取到的tag不完整的问题](#beatifulsoup中获取到的tag不完整的问题)
- [3. 简单的数据处理过程](#3-简单的数据处理过程)
- [4. 便捷网络数据获取](#4-便捷网络数据获取)
  - [4.1. csv文件转换成DataFrame](#41-csv文件转换成dataframe)
  - [4.2. 使用第三方的API进行获取数据](#42-使用第三方的api进行获取数据)
- [5. 数据准备](#5-数据准备)
  - [5.1. 对于数据缺省的处理](#51-对于数据缺省的处理)
    - [5.1.1. 概述](#511-概述)
    - [5.1.2. 拉格朗日插值法](#512-拉格朗日插值法)
- [6. 数据整理](#6-数据整理)
- [7. 数据显示](#7-数据显示)
- [8. 数据选择](#8-数据选择)
- [9. 简单统计与处理](#9-简单统计与处理)
- [10. Grouping](#10-grouping)
  - [10.1. 函数](#101-函数)
- [11. 数据存取](#11-数据存取)
  - [11.1. 存入csv](#111-存入csv)
  - [11.2. 读出csv](#112-读出csv)
  - [11.3. Excel文件存取](#113-excel文件存取)

<!-- /TOC -->
1. 本地数据获取

# 1. 文件的打开，读写和关闭
1. 打开后才能进行读写
2. 读文件
3. 写文件
4. 文件为什么需要关闭?因为python会缓存写入数据，如果出现异常，会导致不能写入。

## 1.1. 文件的打开
1. 函数`file_obj = open(filename,mode='r',buffering = -1,...)`
2. mode为可选参数，默认为r
3. buffering也是可选参数，如果为-1，使用默认系统缓存区，如果为0，不缓冲。如果大于等于1表示缓冲相应行数。
    + r:read
    + w:write，清空写
    + a:在文件尾部追加。
    + 如果之后加b是二进制文件的读写追加。
4. 其他用help(open)来查看
5. python中二进制文件不需要缓冲，文本文件必须缓冲。
6. open返回一个文件对象，可以迭代的
    1. 有很多文件方法
    2. f.read(size):读出至多size字节数据，返回字符串。
    3. f.write(str)
    4. f.readline()
    5. f.readlines()
        + 返回一个列表，会包含\n
        + 根据文件指针来读取
    6. f.writelines()
    7. f.close()
    8. f.seek(offset,whence = 0)
        + 在文件中移动文件指针，从whence(0是文件头部，1是当前位置，2是文件尾部)偏移offset个字节
    9. f.read():从读入位置一直读到结尾。会最后执行close的行为。

## 1.2. 标准文件
1. 程序启动后，以下标准文件很有效
    1. stdin:标准输入
    2. stdout:标准输出
    3. stderr:标准错误
2. input和print都是sys模块的具体实现

# 2. 网络数据获取
1. 网页会动态的更新数据
2. 抓取网页，解析网页内容
3. 第三方API进行抓取解析

## 2.1. 抓取
1. urllib内建模块
    + urllib.request
2. Requests第三方库(小型爬虫开发)
    1. 简单、方便和人性化的Python HTTP第三方库
    2. 基本方法:`requests.get()`等
        + 请求获取指定URL位置的资源，对应HTTP协议的GET方法。
    3. <a href = "https://2.python-requests.org/en/master/">官网</a>
    4. pip install这个第三方库
    5. 利用数据集、API等方式来抓取动态数据
    6. json解码器解码:`.json()`
    7. 二进制解码:`.content()`
3. Scrapy框架(大型爬虫开发)
4. 抓取网页需要注意他的爬虫协议:
    1. 一般是在/robots.txt
        + 比如"https://www.douban.com/robots.txt"
    2. 写了禁止被抓取的网页
    3. 写明延时

### 2.1.1. 抓取编码出现问题的解决方案
```py
r = requests.get(URL)
result = r.text.encode('iso-8859-1').decode('utf-8')
```
1. 统一把字符集转换成unicode之后再使用utf-8字符集解码

## 2.2. 解析
1. Beautiful Soup库
    1. 是一个HTML或XML库中提取解析数据的库
    2. <a href = "https://www.crummy.com/software/BeautifulSoup/">官网</a>
    3. 常用Ixml进行HTML进行解析
    4. 生成一个soup对象
        + `BeautifulSoup(str,"lxml")`
        + tag:HTML或XML文档中的标签
            1. name:`tag.name`
            2. attribute:`tag.attrs`类似于字典
        + NavigableString是Tag当中的字符串
        + comment是NavigableString的一个子类
    5. 获得相应对象:`soup.(tag_name)`
    6. 寻找所有内容:`soup.find_all(tagname,classname)`就可以找到所有tag的name是name的

```python
from bs4 import requests
r = requests.get('url')
soup = BeautifulSoup(r.text,'lxml')
pattern = soup.fina_all('p','comment-content')
for item Sin pattern:
    print(item.string)
```
```python
import requests
from bs4 import BeautifulSoup

r = requests.get("https://book.douban.com/subject/33462956/?icn=index-latestbook-subject")
soup = BeautifulSoup(r.text,'lxml')
pattern = soup.find_all('span','short')
# print(pattern)
for x in pattern:
    print(x.string)
```

2. re模块：正则表达式
```python
pattern = re.compile("1(.*?)2")
p = re.findall(pattern,t.text)
for star in p:
    sum += int(star)
```

## BeatifulSoup中获取到的tag不完整的问题
1. 因为html文件的不规范导致了问题
2. `soup = BeautifulSoup(text,'html.parser')`

# 3. 简单的数据处理过程
1. 分为四个部分:
    1. 数据收集
    2. 数据整理
    3. 数据描述
    4. 数据分析

# 4. 便捷网络数据获取
1. 在网站上可以下载的数据
2. 一般下载格式是csv或json

## 4.1. csv文件转换成DataFrame
1. `pd.read_csv(name.csv)`

## 4.2. 使用第三方的API进行获取数据
1. 直接获取数据
2. 使用`r = requests.get(url)`
    + url参见api相关解析
3. json格式解码:`r.json()`

# 5. 数据准备
1. 加入名称索引等工作
2. 数据准备（预处理）过程中常常需要进行数据的处理，例如数据清洗包括缺失值和异常值处理，数据变换如规范化数据，数据规约如属性规约（选择部分有代表性的属性）等，在Python有很多进行数据预处理的快速方法，以数据清洗中的缺失值处理为例，在实际过程中常常会发现有的数据是缺失（NaN）的，这些值是需要特别处理的。


## 5.1. 对于数据缺省的处理

### 5.1.1. 概述
+ 缺失值的判断可利用numpy中的isnan()函数，而对于Series或DataFrame，缺失值的判断和处理非常的方便，例如df.dropna()可以删掉含NaN(NA)的行，df.dropna(how='all')只丢弃全为NaN的那些行，也可以进行值的插补，例如用0、均值、中位数或众数等进行填充插补，也可用插值法即基于已知点建立插值函数f(x)，通过xi求得f(xi)来近似替代，常用方法有拉格朗日插值法和牛顿插值法NaN，具体插值例子请见下周6.5节。以常用的简单填充为例，可用df.fillna(某一个值)方式用某一个值如0或平均值等代替NaN（例如df.fillna(0)表示用0代替NaN），也可用其method参数指定缺失值的填充方向
    + ffill表示用前一个非缺失值代替NaN填充，
    + bfill表示用下一个非缺失数据代替NaN填充，要根据数据的特点选择不同的填充方式
```python
fruit_df = pd.Series(['apple', 'orange', 'pear'], index=[0, 2, 5])
>>> fruit_df = fruit_df.reindex(range(7))
>>> fruit_df
0     apple
1       NaN
2    orange
3       NaN
4       NaN
5      pear
6       NaN
dtype: object
# inplace参数设为True表示直接修改原对象fruit_df，否则将填充后的结果返回，原对象不变
fruit_df.fillna(method='ffill', inplace = True) 
print(fruit_df)
0     apple
1     apple
2    orange
3    orange
4    orange
5      pear
6      pear
dtype: object
```


### 5.1.2. 拉格朗日插值法
可以使用插值函数对缺失值进行插补，例如拉格朗日插值法，它的基本思想是对于平面上已知的n个点确定一个n-1次多项式使此多项式曲线过这n个点，然后将缺失的函数值对应的点x带入多项式得到缺失值的近似值L(x)。请有兴趣的小伙伴（其他小伙伴可跳过）试着理解如下实现此功能的程序。
```python
import pandas as pd
from scipy.interpolate import lagrange # 导入拉格朗日插值函数
 
df = pd.read_excel("nan.xlsx")
for i in df.columns:
  for j in range(len(df)):
      if (df[i].isnull())[j]: # 如果为空则进行插值
        k = 3  # 设置取前后数的个数为3，默认为5
        y = df[i][list(range(j-k, j)) + list(range(j+1, j+1+k))] # 取数
        y = y[y.notnull()]  # 去掉取出数中的空值
        df[i][j] = lagrange(y.index, list(y))(j)
 print(df
```

# 6. 数据整理
1. 行索引、列索引的变化
2. 给DataFrame添加列索引
3. 处理时间序列:
    1. `x = data.fromtimestamp(...)`
    2. `strftime(x,'%Y-%m-%d')`
    3. 删除原来列:`frame.drop(['data'],axis = 1)`
    4. pandas常见时间序列:`pd.data_range('20170620',period = 7)`
        + period是一共几天
    
# 7. 数据显示
1. 防止一开始的数据错误等问题
2. 常用的显示方式:
    1. 显示行索引`frame.index`
    2. 显示列索引`frame.columns`
    3. 显示数据的值`frame.values`
    4. 显示数据描述`frame.describe()`
    5. 显示数据格式`frame.column_name => dtype`
    6. 行显示:`frame.head(number)|frame.tail(number)`用来查看前几行或者后几行
        + 也可以用传统的切片来完成`frame[:5]`

# 8. 数据选择
1. 选择行
    1. 切片
    2. 索引:使用索引值来进行
2. 选择列
    1. 列名，不支持多列选择
    2. 使用`.loc(axis0,axis1['name'],...)`标签用来选择类，全部选择写一个冒号即可
    3. 选择一个值:`.at[行,列]`
3. 选择区域
4. 筛选(条件选择)
    1. 条件写在中括号中即可
    2. `frame[(a>1)&(a<3)]`
5. `.iloc[]`:使用物理位置来选择位置，使用数字
6. `.iat[]`:参考上面一点
7. `.ix[]`:是以上两者的混合
    + `frame.ix['name'/number]`:返回相应列的所有值
    + `frame.ix[:,['name'/number]]`:进行相应的筛选，如果'b':'d':那么含最后一项而数字的话就不含

# 9. 简单统计与处理
1. 求平均值:`frame.name.mean()`
2. 获得名字:`frame.name.name`
3. 统计上涨和下跌:`np.sign(np.diff(frame.close))`
    + 1是上涨，-1是下降
    + `np.where()`查找
4. 排序:
    1. 按照values排列:`frame.sort_values(by = column_name,ascending = false(逆序))`
    2. 按照index进行排列:`frame.sort_index(by = column_name,ascending = false(逆序))`
5. 计数统计:
    1. 先筛选，再用len
    2. `time.strptime(str,%Y-%m-%d)`将一个字符串格式化为一个time信息
    3. 然后访问上述创建的对象的相应参数来完成
        + `.value_counts()`
    4. groupby()也是一个好方法

# 10. Grouping
1. 分类汇总，现将数据分为几个小组，分别进行处理后进行汇总
2. 调整数据来完成简化运算
3. 问题主要是确定字段

## 10.1. 函数
1. `frame.groupby('name')`:按照name所在类进行分类
2. 可以用`.count()`来统计时间
3. 可以用`.sum()`来求和

# 11. 数据存取
1. 除了使用标准的输入输出文件，常用csv来存取

## 11.1. 存入csv
1. DataFrame转换为csv:`frame.to_csv(file_name)`
2. csv用txt打开会用,进行分割

## 11.2. 读出csv
1. DataFrame读取csv:`pandas.read_csv(file_name)`

## 11.3. Excel文件存取
1. DataFrame写入语法:`frame.to_excel(file_name,sheet_name = )`
2. 读取文件:`pandas.read_excel(filename)`
