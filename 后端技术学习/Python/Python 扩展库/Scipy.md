<!-- TOC -->

- [1. SciPy](#1-scipy)
  - [1.1. 包含的SciPy中的数据结构](#11-包含的scipy中的数据结构)
  - [1.2. SciPy核心库](#12-scipy核心库)
  - [1.3. 函数](#13-函数)
- [2. shift](#2-shift)
  - [2.1. scipy.ndimage.interpolation.shift](#21-scipyndimageinterpolationshift)
  - [2.2. 官方文档](#22-官方文档)

<!-- /TOC -->

# 1. SciPy
1. 是一各基于python的软件生态圈。
2. 主要是为数学、科学和工程服务
3. 一共有6个库，主要介绍前三个:
    1. NumPy
    2. Matplotlib
    3. pandas
    4. Sympy
    5. IPython
    6. Scipy library
4. <a href="https://www.scipy.org/">SciPy官网</a>
    1. <a href ="https://www.numpy.org/devdocs/user/quickstart.html">NumPy手册</a>
    2. <a href ="https://matplotlib.org/gallery/index.html">Matplotlib example</a>


## 1.1. 包含的SciPy中的数据结构
1. ndarray:N维数组
2. Series:变长字典
3. DataFrame:数据框

## 1.2. SciPy核心库
1. 有效计算numpy矩阵，让NumPy和SciPy协同工作
2. 致力于科学计算中的常见问题的各个工具箱，其不同子模块有不同的应用，如插值、积分、优化和图像处理等。

## 1.3. 函数
1. 导入:`from scipy import linalg`
2. `linalg.det(arr)`计算行列式的值

# 2. shift
1. 用来进行数据增强提高模型性能。
2. <a href = "https://docs.scipy.org/doc/scipy/reference/generated/scipy.ndimage.shift.html">官方文档</a>

## 2.1. scipy.ndimage.interpolation.shift
1. 完成数组偏移,是对图像进行增强一种手段
```py
from scipy.ndimage.interpolation import shift
a = np.array([[1,2],[3,4]])
#array([[1, 2],
#       [3, 4]])
shift(a,shift=[1,1],cval=1)
#array([[1, 1],
#       [1, 1]])
```
1. 第一个参数为输入，应为数组，
2. 第二个shift参数表示各个维度的偏移量[1,1]表示第一个第二个维度均偏移1，
3. 第三个参数cval表示偏移后原来位置以什么值填充。

## 2.2. 官方文档
1. `scipy.ndimage.interpolation.shift(input, shift, output_type=None, output=None, order=3, mode='constant', cval=0.0, prefilter=True)`
2. 描述:The array is shifted using spline interpolation of the requested order. Points outside the boundaries of the input are filled according to the given mode. The parameter prefilter determines if the input is pre-filtered before interpolation, if False it is assumed that the input is already filtered.(使用请求顺序的样条插值来移动阵列。输入边界外的点根据给定模式填充。参数prefilter确定输入是否在插值前预过滤，如果为False，则假定输入已过滤。)