Python 手写体识别
---

# 1. Keras 实现手写体识别
```py
# 导入Keras
import keras
# 从keras中导入mnist数据集
from keras.datasets import mnist
# 导入序贯模型
from keras.models import Sequential
# 导入全连接层
from keras.layers import Dense
# 导入优化函数
from keras.optimizers import SGD

# 训练
if __name__ == '__main__':
    # 下载mnist数据集
    (x_train, y_train), (x_test, y_test) = mnist.load_data()

    # 将图片摊平，变成向量
    x_train = x_train.reshape(60000, 784)
    # 对测试集进行同样的处理
    x_test = x_test.reshape(10000, 784)

    x_train = x_train / 255
    x_test = x_test / 255
    y_train = keras.utils.to_categorical(y_train,10)
    y_test = keras.utils.to_categorical(y_test,10)

    # 构建一个空的序贯模型
    model = Sequential()
    # 添加神经网络层
    model.add(Dense(512,activation='relu',input_shape=(784,)))
    model.add(Dense(256,activation='relu'))
    model.add(Dense(10,activation='softmax'))
    model.summary()
    model.compile(optimizer=SGD(),loss='categorical_crossentropy',metrics=['accuracy'])

    # 此处直接将测试集用作了验证集
    model.fit(x_train, y_train, batch_size=64,
              epochs=100, validation_data=(x_test,y_test))
    model.save_weights("./model/my_mnist_weights.h5")
    score = model.evaluate(x_test,y_test)
    print("loss:",score[0])
    print("accu:",score[1])
```

# 2. 参考
1. <a href = "https://blog.csdn.net/louishao/article/details/60867339">Python(TensorFlow框架)实现手写数字识别系统</a>