**genism**

<!-- TOC -->

- [1. 什么是genism](#1-什么是genism)
- [2. 训练模型](#2-训练模型)
  - [2.1. 训练](#21-训练)
  - [2.2. 模型导出](#22-模型导出)
  - [2.3. 模型导入](#23-模型导入)
  - [2.4. 增量训练](#24-增量训练)
- [3. 参考](#3-参考)

<!-- /TOC -->

# 1. 什么是genism
1. genism是强大的自然语言处理工具，里面包括N多常见模型
2. 基本的语料处理工具
    + LSI
    + LDA
    + HDP
    + DTM
    + DIM
    + TF-IDF

# 2. 训练模型

## 2.1. 训练
```py
#encoding=utf-8
from gensim.models import word2vec
sentences=word2vec.Text8Corpus(u'分词后的爽肤水评论.txt')
model=word2vec.Word2Vec(sentences, size=50)

y2=model.similarity(u"好", u"还行")
print(y2)

for i in model.most_similar(u"滋润"):
    print i[0],i[1]
```
1. 训练模型:`model=word2vec.Word2Vec(sentences,min_count=5,size=50)`
    + 第一个参数是训练语料，第二个参数是小于该数的单词会被剔除，默认值为5。
    + 第三个参数是神经网络的隐藏层单元数，默认为100

## 2.2. 模型导出
```py
word2vec = gensim.models.word2vec.Word2Vec(sentences(), size=256, window=10, min_count=64, sg=1, hs=1, iter=10, workers=25)
word2vec.save('word2vec_wx')
```

## 2.3. 模型导入
```py
model = gensim.models.Word2Vec.load('xxx/word2vec_wx')
pd.Series(model.most_similar(u'微信',topn = 360000))
```
1. gensim.models.Word2Vec.load的办法导入
其中的Numpy,可以用numpy.load：
```py
import numpy
word_2x = numpy.load('xxx/word2vec_wx.wv.syn0.npy')
```
2. 导入方式
```py
from gensim.models.keyedvectors import KeyedVectors
word_vectors = KeyedVectors.load_word2vec_format('/tmp/vectors.txt', binary=False)  # C text format
word_vectors = KeyedVectors.load_word2vec_format('/tmp/vectors.bin', binary=True)  # C binary format
```

## 2.4. 增量训练
```py
model = gensim.models.Word2Vec.load('/tmp/mymodel')
model.train(more_sentences)
```

# 3. 参考
1. <a href = "https://blog.csdn.net/gdh756462786/article/details/79108665/">genism</a>