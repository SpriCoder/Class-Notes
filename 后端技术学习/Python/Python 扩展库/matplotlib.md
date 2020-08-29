**Matplotlib**
<!-- TOC -->

- [1. Matplotlib绘图基础](#1-matplotlib绘图基础)
- [2. 绘图API-pyplot模块](#2-绘图api-pyplot模块)
  - [2.1. 数据源](#21-数据源)
  - [2.2. 绘制折线图](#22-绘制折线图)
  - [2.3. 绘制散点图Scatter](#23-绘制散点图scatter)
  - [2.4. 绘制柱状图](#24-绘制柱状图)
    - [2.4.1. 基本柱状图](#241-基本柱状图)
    - [2.4.2. 设置颜色](#242-设置颜色)
    - [2.4.3. 设置标签](#243-设置标签)
    - [2.4.4. 堆叠柱状图](#244-堆叠柱状图)
    - [2.4.5. 并列柱状图](#245-并列柱状图)
    - [2.4.6. 条形柱状图](#246-条形柱状图)
    - [2.4.7. 参考](#247-参考)
  - [2.5. 绘制饼状图](#25-绘制饼状图)
    - [2.5.1. 饼图代码实践](#251-饼图代码实践)
    - [2.5.2. 部分选项扩展](#252-部分选项扩展)
    - [2.5.3. 参考](#253-参考)
  - [2.6. 绘制堆叠图](#26-绘制堆叠图)
  - [2.7. 绘制箱线图](#27-绘制箱线图)
  - [2.8. 绘制直线图](#28-绘制直线图)
  - [2.9. 绘制雷达图](#29-绘制雷达图)
  - [2.10. 集成库-pylab模块(包含Numpy和pyplot中的常用函数)](#210-集成库-pylab模块包含numpy和pyplot中的常用函数)
- [3. Matplotlib图像属性](#3-matplotlib图像属性)
  - [3.1. 色彩和样式](#31-色彩和样式)
  - [3.2. 文字](#32-文字)
  - [3.3. 其他属性](#33-其他属性)
    - [3.3.1. 绘图的全局rc变量的设置](#331-绘图的全局rc变量的设置)
    - [3.3.2. figure](#332-figure)
    - [3.3.3. 线条属性](#333-线条属性)
    - [3.3.4. xticks & yticks](#334-xticks--yticks)
    - [3.3.5. xlim & ylim](#335-xlim--ylim)
    - [3.3.6. 多子图-subplots](#336-多子图-subplots)
    - [3.3.7. 子图-axes](#337-子图-axes)
- [4. pandas作图](#4-pandas作图)
  - [4.1. pandas绘图](#41-pandas绘图)
  - [4.2. 控制图形形式](#42-控制图形形式)
  - [4.3. 箱型图(盒区图)](#43-箱型图盒区图)
- [5. squarify](#5-squarify)
- [6. Seaborn](#6-seaborn)
  - [6.1. heatmap](#61-heatmap)
- [7. matplotlib.pyploy(plt)的常见方法等介绍](#7-matplotlibpyployplt的常见方法等介绍)
- [8. Matplotlib中的中文乱码解决](#8-matplotlib中的中文乱码解决)

<!-- /TOC -->
# 1. Matplotlib绘图基础
1. 基于NumPy
2. 二维绘图库，简单快速地生成曲线图、直方图和散点图等形式的图。
3. 常用的pyplot模块是一个简单提供类似MATLAB接口的模块

# 2. 绘图API-pyplot模块
1. 调用其中函数进行绘图即可。

## 2.1. 数据源
1. 一个Dataframe

## 2.2. 绘制折线图
 
1. 可以保存图片`plt.savefig('name.格式')`
```python
import matplotlib.pyplot as plt
x = frame.index
y = frame.values
plt.plot(x,y)# 基础画图元素，基本两个元素x和y
```
2. NumPy数组也可以作为Matplotlib的参数
```python
import numpy as np
import matplotlib.pyplot as plt
t = np.arange(0.,4.,0.1)
plt.plot(t,t,t,t+2,t,t**2)# 多组x,y联合解析
```
3. 可以直接用Series进行绘图

## 2.3. 绘制散点图Scatter
1. 绘制方法`plt.plot(t,t,'o')`
    + 添加参数o即可
2. 使用`plt.scatter(x,y)`进行绘制

## 2.4. 绘制柱状图
1. 绘制方法:`plt.bat(x,y)`
    + width:设置柱状图的宽度
    + yerr:柱状图中的误差区间
    + bottom:

### 2.4.1. 基本柱状图
```py
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt

num_list = [1.5,0.6,7.8,6]
plt.bar(range(len(num_list)), num_list)
plt.show()
```

### 2.4.2. 设置颜色
```py
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt

num_list = [1.5,0.6,7.8,6]
plt.bar(range(len(num_list)), num_list,fc='r')
plt.bar(range(len(num_list)), num_list,color='rgb')# color中标识按照这个颜色顺序上色
plt.show()
```

### 2.4.3. 设置标签
```py
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt

name_list = ['Monday','Tuesday','Friday','Sunday']
num_list = [1.5,0.6,7.8,6]
plt.bar(range(len(num_list)), num_list,color='rgb',tick_label=name_list)
plt.show()
```

### 2.4.4. 堆叠柱状图
```py
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt

name_list = ['Monday','Tuesday','Friday','Sunday']
num_list = [1.5,0.6,7.8,6]
num_list1 = [1,2,3,1]
plt.bar(range(len(num_list)), num_list, label='boy',fc = 'y')
plt.bar(range(len(num_list)), num_list1, bottom=num_list, label='girl',tick_label = name_list,fc = 'r')
plt.legend()
plt.show()
```

### 2.4.5. 并列柱状图
```py
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt

name_list = ['Monday','Tuesday','Friday','Sunday']
num_list = [1.5,0.6,7.8,6]
num_list1 = [1,2,3,1]
x =list(range(len(num_list)))
total_width, n = 0.8, 2
width = total_width / n

plt.bar(x, num_list, width=width, label='boy',fc = 'y')
for i in range(len(x)):
    x[i] = x[i] + width
plt.bar(x, num_list1, width=width, label='girl',tick_label = name_list,fc = 'r')
plt.legend()
plt.show()
```

### 2.4.6. 条形柱状图
```py
# -*- coding: utf-8 -*-
import matplotlib.pyplot as plt

name_list = ['Monday','Tuesday','Friday','Sunday']
num_list = [1.5,0.6,7.8,6]
plt.barh(range(len(num_list)), num_list,tick_label = name_list)
plt.show()
```

### 2.4.7. 参考
1. <a href = "https://www.cnblogs.com/always-fight/p/9707727.html">柱状图</a>
2. <a href = "https://blog.csdn.net/rose_424/article/details/78340390">柱状图的其他部分参数</a>
3. <a href = "https://blog.csdn.net/qq_29721419/article/details/71638912?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task">各种柱状图画法</a>

## 2.5. 绘制饼状图
1. 绘制方法:`pie(x, explode=None, labels=None, colors=None, autopct=None,pctdistance=0.6, shadow=False, labeldistance=1.1, startangle=None,radius=None, counterclock=True, wedgeprops=None, textprops=None,center=(0, 0), frame=False, rotatelabels=False, hold=None, data=None)`
    + x:(每一块)的比例，如果sum(x) > 1会使用sum(x)归一化；
    + labels  :(每一块)饼图外侧显示的说明文字；
    + explode :(每一块)离开中心距离；
    + startangle :起始绘制角度,默认图是从x轴正方向逆时针画起,如设定=90则从y轴正方向画起；
    + shadow :在饼图下面画一个阴影。默认值：False，即不画阴影；
    + labeldistance :label标记的绘制位置,相对于半径的比例，默认值为1.1, 如<1则绘制在饼图内侧；
    + autopct :控制饼图内百分比设置,可以使用format字符串或者format function '%1.1f'指小数点前后位数(没有用空格补齐)；
    + pctdistance :类似于labeldistance,指定autopct的位置刻度,默认值为0.6；
    + radius  :控制饼图半径，默认值为1；
    + counterclock ：指定指针方向；布尔值，可选参数，默认为：True，即逆时针。将值改为False即可改为顺时针。
    + wedgeprops ：字典类型，可选参数，默认值：None。参数字典传递给wedge对象用来画一个饼图。例如：wedgeprops={'linewidth':3}设置wedge线宽为3。
    + textprops ：设置标签（labels）和比例文字的格式；字典类型，可选参数，默认值为：None。传递给text对象的字典参数。
    + center ：浮点类型的列表，可选参数，默认值：(0,0)。图标中心位置。
    + frame ：布尔类型，可选参数，默认值：False。如果是true，绘制带有表的轴框架。
    + rotatelabels ：布尔类型，可选参数，默认为：False。如果为True，旋转每个label到指定的角度。
2. <a href = "https://www.cnblogs.com/biyoulin/p/9565350.html">饼图精讲</a>

### 2.5.1. 饼图代码实践
```py
#读者分布
import pandas as pd
import matplotlib.pyplot as plt

def age_pie():
    plt.rcParams['font.family'] = 'SimHei'
    df = pd.read_csv("user.csv", sep=";", header=None, names=["user_id", "location", "age"],
                     encoding='gbk')
    # print(df.head())
    df[(df.age == '41"')].index.tolist()  # 找出不合法数值所在行
    df = df.drop([1305])  # 删除那一行
    df[['age']] = df[['age']].astype(float)
    labels = ['10-20', '20-30', '30-40', '40-50', '50-60']
    sizes = [len(df[(df.age >= 10) & (df.age < 20)]), \
             len(df[(df.age >= 20) & (df.age < 30)]), \
             len(df[(df.age >= 30) & (df.age < 40)]), \
             len(df[(df.age >= 40) & (df.age < 50)]), \
             len(df[(df.age >= 50) & (df.age < 60)])]
    # print(sizes)
    explode = (0, 0.05, 0, 0, 0)  # 0.1为第二个元素凸出距离
    colors = ['tomato', 'lightskyblue', 'goldenrod', 'green', 'y']
    # 饼图绘制函数
    plt.figure(figsize=(8, 6))
    plt.pie(sizes, explode=explode, labels=labels, colors=colors, \
            autopct='%1.1f%%', shadow=False, pctdistance=0.8, \
            startangle=90, textprops={'fontsize': 16, 'color': 'w'})
    plt.title('读者年龄分布图')
    plt.axis('equal')
    plt.legend(loc='upper right') # 显示图例
    plt.savefig('age.png', dpi=600)
    plt.show()
print(age_pie())
```

### 2.5.2. 部分选项扩展
1. 显示正负号
```py
#用来正常显示负号  
plt.rcParams['axes.unicode_minus']=False 
```
2. 显示中文
```
plt.rcParams['font.family'] = 'SimHei'
黑体  SimHei  
微软雅黑    Microsoft YaHei  
微软正黑体   Microsoft JhengHei  
新宋体 NSimSun  
新细明体    PMingLiU  
细明体 MingLiU  
标楷体 DFKai-SB  
仿宋  FangSong  
楷体  KaiTi  
仿宋_GB2312   FangSong_GB2312  
楷体_GB2312   KaiTi_GB2312    
```

### 2.5.3. 参考
1. <a href = "https://blog.csdn.net/proplume/article/details/80768705?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task">Python可视化: 颜色 图例 实例（matplotlib饼状图）总结</a>

## 2.6. 绘制堆叠图
1. plt.stackplot(x,y, labels=labels, colors=pal, alpha=0.7 )
2. <a href = "https://blog.csdn.net/qq_35189715/article/details/96108580">堆叠图</a>

## 2.7. 绘制箱线图
1. 箱线图是由最高位最低位和中位以及上下四分位绘制的图片。
2. <a href ="https://www.cnblogs.com/wyy1480/p/9526264.html">箱线图boxplot</a>

## 2.8. 绘制直线图
1. 使用plot函数

## 2.9. 绘制雷达图
```py
import numpy as np
import matplotlib.pyplot as plt
import matplotlib
# 设置字体
matplotlib.rcParams['font.family'] = 'SimHei'
matplotlib.rcParams['font.sans-serif'] = ['SimHei']
# 自定义标签
lables = np.array(['综合','KDA','发育','推进','生存','输出'])
# 设置雷达图维度数量
nAttr = 6
# 设置标签对应的值
date = np.array([7, 5, 6, 9, 8, 7])
# 获取到每一个对应的角度
angles = np.linspace(0, 2*np.pi, nAttr, endpoint=False)
# 拼接数据保证闭环
date = np.concatenate((date, [date[0]]))
angles = np.concatenate((angles, [angles[0]]))
# 开始绘图
fig = plt.figure(facecolor="white")
plt.subplot(111, polar=True)
plt.plot(angles, date, 'bo-', color = 'g', linewidth = 2)
plt.fill(angles, date, facecolor = 'g', alpha = 0.25)
plt.thetagrids(angles*180/np.pi, lables)
plt.figtext(0.52, 0.95, 'DOTA能力值雷达图', ha='center')
plt.grid(True)
plt.savefig('dota_radar.JPG')
plt.show()
```

><a href = "https://www.cnblogs.com/walkwaters/p/12094284.html">雷达图绘制</a>

## 2.10. 集成库-pylab模块(包含Numpy和pyplot中的常用函数)
1. 导入模块:`import pylab as pl`

# 3. Matplotlib图像属性
1. 允许定制图片的各项性质
2. 几乎可以控制所有的值

## 3.1. 色彩和样式
1. 默认格式:`b-`:蓝色+实线
2. 其他格式:
    1. `g--`:绿色+虚线
    2. `rD`:红色+钻石
3. 标记和符号类似MATLAB
4. 使用hatch来确定填充模块问题

符号/线条|颜色/描述
--|--
b|blue
g|green
r|red
c|cyan
m|magenta
y|yellow
k|black
w|white
'-'|solid
'--'|dashed
'-.'|dash_dot
':'|dotted
'None'|draw nothing
''|draw nothing
' '|draw nothing
"o"|circle
"v"|triangle_down
"s"|square
"p"|pentagon
"*"|star
"h"|hexagon1
"+"|plus
"D"|diamond

4. 具体可以通过help进行查看
5. markersize是上述标记的大小

## 3.2. 文字
1. 题目:`plt.title(str)`
2. x轴标签:`plt.xlabel(str)`
3. y轴标签:`plt.tlabel(str)`

## 3.3. 其他属性

### 3.3.1. 绘图的全局rc变量的设置
1. pylot使用rc配置文件来自定义图形的各种默认属性，称之为rc配置或rc参数。通过rc参数可以修改默认的属性，包括窗体大小、每英寸的点数、线条宽度、颜色、样式、坐标轴、坐标和网络属性、文本、字体等。
2. rc参数存储在字典变量中，通过字典的方式进行访问

### 3.3.2. figure
1. `pl.figure(figsize = (8,6) ,dpi = 100)`
    + figsize是图的大小
    + dpi是图片精度

### 3.3.3. 线条属性
1. `pl.plot(t,t,color = "blue",linestyle = "-",marker = "+",linewidth = 3,label = "Line2")`
    + color表示颜色
    + linestyle表示线形状
    + marker就是点的形状
    + linewidth是线宽
    + label是在线板上的名称
    + 可以多次plot绘制曲线，未必在一个函数中写完
    + cumulative:对数据进行列举
2. pl.legend(loc = "upper left")
    + 放置图例的位置

### 3.3.4. xticks & yticks
1. 设置横轴和纵轴的精度。`plt.xticks(x,xticks,siz e = "small",rotation = 30)`

### 3.3.5. xlim & ylim
1. 用来设置x轴和y轴的范围。
2. `plt.ylim(0,3700)`

### 3.3.6. 多子图-subplots
1. `plt.subplot(number)`
    + number是三个数字
    + 第一个参数是行数
    + 第二个参数是列数
    + 第三个参数是所在区域的编号
2. 多图绘制在一张图上
    + 首先用subplot指定图`plt.subplot(211)`
    + 然后绘制图`plt.plot()`
3. 调整多子图之间距离
    + 调整垂直距离`plt.subplots_adjust(hspace = 0.5)`
    + 调整水平距离`plt.subplots_adjust(vspace = 0.5)`

```py
# 具体示例
#!/usr/bin/env python
#!encoding=utf-8

import matplotlib
import matplotlib.pyplot as plt

if __name__ == '__main__':
    for i,color in enumerate("rgby"):
        plt.subplot(221+i, axisbg=color)

    plt.show()
```

### 3.3.7. 子图-axes
1. 格式`axes([left,bottom,width,height])`
    + left和左边界的距离
    + bottom和底边界的距离
    + width是图的高度(按照百分比计算)
    + height是图的宽度(按照百分比计算)
    + 参数范围(0,1)
2. 这个是对于子图的属性进行操作。

# 4. pandas作图
1. 基于Series和DataFrame的作图

## 4.1. pandas绘图
```python
import pandas as pd
closeMeansKO.plot()
```
1. 可以联合以上手段进行联合绘制`Series.column_name.plot()`

## 4.2. 控制图形形式
```python
df = pd.DataFrame()
df['1']=volumn1
df['2']=volumn2
df.plot(kind = 'bar',stacked = True,) # kind表示图的形式，柱状图:bar.stacked表示是否使用堆积效果
df.plot(kind = 'pie',subplots = True,autopct = '%.2f'，marker = 'v')# 饼状图乡音相应参数
```

## 4.3. 箱型图(盒区图)
1. 非常好反映原始数据的分布情况
2. 函数:`df.bocplot()`
3. 从上向下五条线:
    1. 第一条:上边缘,最大值
    2. 第二条:25%位置
    3. 第三条:50%位置
    4. 第四条:75%位置
    5. 第五条:下边缘，最小值

# 5. squarify
1. 只能绘制一级树图
2. <a href = "https://blog.csdn.net/u010665216/article/details/77841199">绘制一级树图</a>

# 6. Seaborn
1. Python中的一种可视化库。

## 6.1. heatmap
1. 热图:反映的是一些数据的热度。
    + <a href = "https://www.jianshu.com/p/363bbf6ec335">Python可视化：Seabon库热力图使用进阶</a>

# 7. matplotlib.pyploy(plt)的常见方法等介绍
方法名|方法|部分参数注解
--|--|--
plot()|绘制直线图|各个变量
show()|展示图，将已经绘制好的图片显示出来|-
scatter()|绘制散点图|各个变量
xlim()|设置x轴的上下限|元组[a,b]表示从a到b
ylim()|设置y轴的上下限|元组[a,b]表示从a到b
legend()|为图像加上图例|<a href = "https://blog.csdn.net/qq_33221533/article/details/81431264">详情</a>
title(str)|添加表的名称|-
xlabel(str)|添加x轴的名称|-
ylabel(str)|添加y轴的名称|-

# 8. Matplotlib中的中文乱码解决
1. 我们可以在绘制图片的时候显式的指定字体。