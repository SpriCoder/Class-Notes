聚类分析
---
1. 聚类是数据挖掘描述性任务和预测性任务的一个重要组成部分，他以相似性为基础，把相似的对象通过静态分类，分成不同的组别和子集。

<!-- TOC -->

- [1. k-mean算法](#1-k-mean算法)
- [2. Scikit-learn](#2-scikit-learn)
  - [2.1. K-Mean算法](#21-k-mean算法)
  - [2.2. 支持向量机算法](#22-支持向量机算法)

<!-- /TOC -->

# 1. k-mean算法
1. 任意选k个对象作为初识的聚类的中心
2. 接着对每个点确定他的聚类中心
    + 一般使用距离，使用均方差作为标准测度函数
3. 接下来确定每个新聚类的聚类中心，直到收敛。
    + 也就是知道确定的中心点不再改变
4. 原则特点:保证各聚类之间尽可能紧凑，各聚类之间尽可能的分开
5. SciPy的cluster模块中有一个vq，一个矢量量化包
6. k-means是局部最优解，而不是全局最优解
```python
import numpy as np
from scipy.cluster.vq import vq,kmeans,whiten
list1 = [1,2,3]
list2 = [2,3,4]
list3 = [3,4,5]
data = np.array([list1,list2,list3])
whiten = whiten(data)# 计算各列的标准差，形成新的数组
centroids,_ = kmeans(whiten,2)# 对数据进行聚类，第二个参数(分为几类)，返回一个元组，第二个_是占位符
result,_ = vq(whiten,centroids)# 是一个矢量量化，对其中的每一个人进行分类获得结果
result => [1,0,0,1,1,0]
```


# 2. Scikit-learn
1. 著名的机器学习包，提供了各种机器学习的接口

## 2.1. K-Mean算法
```python
import numpy as np
from sklearn.cluster import KMeans
list1 = [1,2,3]
list2 = [2,3,4]
list3 = [3,4,5]
data = np.array([list1,list2,list3])
kmeans = KMeans(n_cluster = 2).fit(data)# 对确定的数据解来进行聚类
pred = Kmeans.predict(data)# 根据聚类结果来对数据集进行分类
print(pred)
```

## 2.2. 支持向量机算法
```python
from sklearn import datasets
from sklearn import svm
clf = svm.SVC(gamma = 0.001,C= 100.)
digits = datasets.load_digits()
clf.fit(digits.data[:-1],digits.target[:-1])
clf.predict(digits.data[-1])
```