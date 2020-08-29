Python保存和加载模型
---
1. 众所周知，我们将模型训练完成后，希望能够存储下来，在以后不必再次重复训练模型。
2. 同时在以后使用的时候，希望能够加载已经训练好的模型。

<!-- TOC -->

- [1. Python保存模型](#1-python保存模型)
- [2. Python加载模型](#2-python加载模型)
- [3. 参考](#3-参考)

<!-- /TOC -->

# 1. Python保存模型
```py
from sklearn.externals import joblib
A=[[1,2,3],[4,5,6],[7,8,9]]
B=A+A
print(B)
# 保存模型
joblib.dump(B, 'saved_model/B.pkl')
```

# 2. Python加载模型
```py
from sklearn.externals import joblib
B = joblib.load('saved_model/B.pkl')
print(B)
```

# 3. 参考
1. <a href = "https://blog.csdn.net/xu_xiaoxu/article/details/82976985">Python模型</a>