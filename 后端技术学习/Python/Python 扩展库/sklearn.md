<!-- TOC -->

- [1. sklearn](#1-sklearn)
- [2. TfidVectorizer](#2-tfidvectorizer)
  - [2.1. TF-IDF](#21-tf-idf)
    - [2.1.1. TF](#211-tf)
    - [2.1.2. IDF](#212-idf)
    - [2.1.3. IF-IDF](#213-if-idf)
    - [2.1.4. python中如何进行调用](#214-python中如何进行调用)
- [3. 数据集下载](#3-数据集下载)
  - [3.1. 下载MNIST数据集出现的问题](#31-下载mnist数据集出现的问题)
- [4. 数据预处理的相应操作](#4-数据预处理的相应操作)
  - [4.1. 数据集以及测试集的划分问题](#41-数据集以及测试集的划分问题)
  - [4.2. 数据归一化](#42-数据归一化)
  - [4.3. 数据清洗](#43-数据清洗)
- [5. 进行数据分析](#5-进行数据分析)
  - [5.1. 数据线性回归](#51-数据线性回归)
- [6. 进行评估预测](#6-进行评估预测)
- [7. Scikit-learn 朴素贝叶斯](#7-scikit-learn-朴素贝叶斯)
  - [7.1. GaussianNB](#71-gaussiannb)
  - [7.2. MultinomialNB](#72-multinomialnb)
  - [7.3. BernoulliNB](#73-bernoullinb)
  - [7.4. 朴素贝叶斯的主要优缺点](#74-朴素贝叶斯的主要优缺点)
- [8. 参考](#8-参考)

<!-- /TOC -->
# 1. sklearn
1. 这是一个非常实用的机器学习库

# 2. TfidVectorizer
1. sklearn中的文本特征提取

## 2.1. TF-IDF
1. TF-IDF(term frequency-inverse document frequency)词频-逆向文件频率。
2. 在处理文本时，如何将文字转化为模型可以处理的向量呢？IF-IDF就是这个问题的解决方案之一。字词的重要性与其在文本中出现的频率成正比(IF)，与其在语料库中出现的频率成反比(IDF)。

### 2.1.1. TF
1. TF:词频。IF(w)=(词w在文档中出现的次数)/(文档的总词数)

### 2.1.2. IDF
1. IDF：逆向文件频率。有些词可能在文本中频繁出现，但并不重要，也即信息量小，如is,of,that这些单词，这些单词在语料库中出现的频率也非常大，我们就可以利用这点，降低其权重。
    + IDF(w)=log_e(语料库的总文档数)/(语料库中词w出现的文档数)

### 2.1.3. IF-IDF
IF-IDF = TF * IDF

### 2.1.4. python中如何进行调用
```py
from sklearn.feature_extraction.text import TfidfVectorizer
cv=TfidfVectorizer(binary=False,decode_error='ignore',stop_words='english')
vec=cv.fit_transform(['hello world','this is a panda.'])#传入句子组成的list
arr=vec.toarray()
```
# 3. 数据集下载

## 3.1. 下载MNIST数据集出现的问题
1. <a href = "https://blog.csdn.net/rocwei1001/article/details/97278682">无法下载MNIST数据集</a>

# 4. 数据预处理的相应操作

## 4.1. 数据集以及测试集的划分问题
1. 使用`train_test_split`进行划分数据集
2. `X_train,X_test,Y_train,Y_test = train_test_split(exam_X,exam_Y,train_size=.8)`:将原本数据集拆分成为训练集和测试集

## 4.2. 数据归一化
1. 使用`reshape()`函数来进行数据调整
    1. `reshape(-1,1)`

## 4.3. 数据清洗
```py
#通过read_csv来读取我们的目的数据集
adv_data = pd.read_csv("C:/Users/Administrator/Desktop/Advertising.csv")
#清洗不需要的数据
new_adv_data = adv_data.ix[:,1:]
#得到我们所需要的数据集且查看其前几列以及数据形状
print('head:',new_adv_data.head(),'\nShape:',new_adv_data.shape)
``` 

# 5. 进行数据分析

## 5.1. 数据线性回归
1. 以LinearRegression()为例，我们创建一个model，使用数据进行fit操作
2. 获取运算结果:
    + 截距:`model.intercept`
    + 回归系数:`model.coef`
3. Seabon回归计算:
```py
# 通过加入一个参数kind='reg'，seaborn可以添加一条最佳拟合直线和95%的置信带。
sns.pairplot(new_adv_data, x_vars=['TV','radio','newspaper'], y_vars='sales', size=7, aspect=0.8,kind = 'reg')
plt.savefig("pairplot.jpg")
plt.show()
```

# 6. 进行评估预测
1. 我们导入数据进行预测:`model.predict(X)`

# 7. Scikit-learn 朴素贝叶斯
1. 在Sklearn中一共有三个朴素贝叶斯的分类算法类，分别是GaussianNB、MultinomialNB、BernoulliNB。
    + 他们分别对应的是先验为高斯分布、多项式分布、伯努利分布的朴素贝叶斯。

## 7.1. GaussianNB
1. 主要参数是：先验概率priors
2. 主要预测方法：
    1. predict:直接给出测试集的预测类别输出
    2. predict_proba:给出测试集样本在各个类别上预测的概率
    3. prodict_log_proba:给出测试集样本在各个类别上预测的概率的一个对数转化。
3. fit方法：
    1. fit(X,Y):将相应的输入数据进行匹配
    2. partial_fit():这个方法一般用于数据量过大的时候，用来分词载入内存

## 7.2. MultinomialNB
1. 主要参数:
    1. 参数alpha:无特殊需要，可以选择为1，调优时可以进行局部调整
    2. 布尔参数:fit_prior:表示是否要考虑先验概率,若为false则所有的样本类别输出有相同的先验概率。
    3. class_prior:输入的先验概率

fit_prior|calss_prior|最终的先验概率
--|--|--
false|无意义|P(Y=C<sub>k</sub>) = 1/k
true|不填|P(Y=C<sub>k</sub>) = m<sub>k</sub>
true|填|P(Y=C<sub>k</sub>) = class_prior

## 7.3. BernoulliNB
1. 主要参数:
    1. 前三个参数和MultinomialNB相同
    2. binarize:处理二项分布，可以为数值或者null

## 7.4. 朴素贝叶斯的主要优缺点
1. 优点:
    1. 朴素贝叶斯模型有稳定的分类效率。
    2. 小规模数据表现好，可以处理多分类任务，适合增量学习
    3. 对缺失数据不敏感
2. 缺点:
    1. 理论上:朴素贝叶斯模型与其他分类方法比有最小的误差率。
    2. 需要知道先验概率，影响很大
    3. 先验 + 数据 -> 后验:有一定错误率
    4. 对于输入数据的表达形式比较敏感。

# 8. 参考
1. <a href = "https://www.cnblogs.com/mengnan/p/9307648.html">sklearn文本特征提取——TfidVectorizer</a>
2. <a href = "https://blog.csdn.net/feng_zhiyu/article/details/81952697">Python中的TfidVectorizer参数解析</a>
3. <a href = "https://blog.csdn.net/weixin_40014576/article/details/79918819">Python实现多变量回归</a>