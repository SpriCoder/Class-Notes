keras
---

<!-- TOC -->

- [1. history](#1-history)
- [2. 一些比较实用的函数](#2-一些比较实用的函数)
  - [2.1. pad_sequence函数](#21-pad_sequence函数)
- [3. model搭建](#3-model搭建)
  - [3.1. 文本情感分析例子](#31-文本情感分析例子)
- [4. 参考](#4-参考)

<!-- /TOC -->

# 1. history
1. 在python中，keras通过训练(fit_generator和fit)返回history对象。
2. history对象中记录了输出等信息，所以我们可以显示函数的损失和准确率等信息

# 2. 一些比较实用的函数

## 2.1. pad_sequence函数
```py
pad_sequences = tf.contrib.keras.preprocessing.sequence.pad_sequences
keras.preprocessing.sequence.pad_sequences(sequences,maxlen=None,dtype='int32',padding='pre',truncating='pre', value=0.)
```
1. sequences：浮点数或整数构成的两层嵌套列表
2. maxlen：None或整数，为序列的最大长度。大于此长度的序列将被截短，小于此长度的序列将在后部填0.在命名实体识别任务中，主要是指句子的最大长度
3. dtype：返回的numpy array的数据类型
4. padding：‘pre’或‘post’，确定当需要补0时，在序列的起始还是结尾补
4. truncating：‘pre’或‘post’，确定当需要截断序列时，从起始还是结尾截断
5. value：浮点数，此值将在填充时代替默认的填充值0
6. 返回形如(nb_samples,nb_timesteps)的2D张量

# 3. model搭建

## 3.1. 文本情感分析例子
```py
# 建立模型
model = keras.Sequential()
model.add(keras.layers.Embedding(vocab_size, 16))
model.add(keras.layers.GlobalAveragePooling1D())
model.add(keras.layers.Dense(16, activation=tf.nn.relu))
model.add(keras.layers.Dense(1, activation=tf.nn.sigmoid))

model.summary()
# 编译模型
model.compile(optimizer = 'adam',
              loss='binary_crossentropy',
              metrics=['accuracy'])
# 评估模型
results = model.evaluate(test_data, test_labels)
```
1. Embedding layer(嵌入层)
    + 该层采用整数编码的词汇表，并查找每个词索引的嵌入向量。这些向量是作为模型训练学习的。向量为输出数组添加维度，生成的维度为：(batch, sequence, embedding)。
2. GlobalAveragePooling1D layer(全局平均池化层)
    + 通过对序列维度求平均，为每个评论返回固定长度的输出向量。这允许模型以最简单的方式处理可变长度的输入。
3. Dense layer(全连接层，稠密层)
4. 最后一层和输出节点连接，实用sigmoid激活函数
5. 编译模型:
    1. loss:指的是损失函数(binary_crossentropy在处理概率上表现更好——它测量概率分布之间的“距离”，或者测量真实分布和预测之间的“距离”)


# 4. 参考
1. <a href = "https://blog.csdn.net/yangfengling1023/article/details/81202767">pad_sequences序列预处理</a>
2. <a href = "https://mp.weixin.qq.com/s/Lnn1ypkRD2vRb7KVMV335A">文本情感分析</a>