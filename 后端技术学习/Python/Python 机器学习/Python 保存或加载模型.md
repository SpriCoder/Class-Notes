Python保存和加载模型
---
1. 众所周知，我们将模型训练完成后，希望能够存储下来，在以后不必再次重复训练模型。
2. 同时在以后使用的时候，希望能够加载已经训练好的模型。

<!-- TOC -->

- [1. sklearn](#1-sklearn)
  - [1.1. sklearn保存模型](#11-sklearn保存模型)
  - [1.2. sklearn加载模型](#12-sklearn加载模型)
- [2. Keras](#2-keras)
  - [2.1. 保存h5模型](#21-保存h5模型)
  - [2.2. 加载h5模型](#22-加载h5模型)
  - [2.3. 保存h5模型](#23-保存h5模型)
- [3. 参考](#3-参考)

<!-- /TOC -->

# 1. sklearn

## 1.1. sklearn保存模型
```py
from sklearn.externals import joblib
A=[[1,2,3],[4,5,6],[7,8,9]]
B=A+A
print(B)
# 保存模型
joblib.dump(B, 'saved_model/B.pkl')
```

## 1.2. sklearn加载模型
```py
from sklearn.externals import joblib
B = joblib.load('saved_model/B.pkl')
print(B)
```

# 2. Keras

## 2.1. 保存h5模型
```py

```

## 2.2. 加载h5模型
```py
from keras.models import load_model

model_dir = 'xxx.h5'
model = load_model(model_dir)
model.summary()
```

## 2.3. 保存h5模型
```py
model.save_weights("./model/my_mnist_weights.h5")
```

# 3. 参考
1. <a href = "https://blog.csdn.net/xu_xiaoxu/article/details/82976985">sklearn模型</a>
2. <a href = "https://blog.csdn.net/WILDCHAP_/article/details/107747971">如何加载保存好的keras神经网络h5模型</a>