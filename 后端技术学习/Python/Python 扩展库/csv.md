python csv文件处理
---
<!-- TOC -->

- [1. 什么是csv文件](#1-什么是csv文件)
- [2. python处理csv文件的扩展库](#2-python处理csv文件的扩展库)
  - [2.1. 使用PythonI/O写入csv文件](#21-使用pythonio写入csv文件)
  - [2.2. 使用PythonI/O读出csv文件](#22-使用pythonio读出csv文件)
  - [2.3. 使用Pandas读取CSV文件](#23-使用pandas读取csv文件)
  - [2.4. 使用Tensorflow读取CSV文件](#24-使用tensorflow读取csv文件)
- [3. 处理csv文件中遇到的问题](#3-处理csv文件中遇到的问题)

<!-- /TOC -->
# 1. 什么是csv文件
1. csv文件(又称逗号分隔值文件)，文件以纯文本的方式存储数据(数字和文本)。
2. 特点:
    1. 读取的数据一般为字符类型，如果数字需要人为转换为数字
    2. 以行为单位读取数据
    3. 列之间以半角逗号或制表符为间隔，一般为半角逗号
    4. 一般为每行开头不空格，第一行为属性列，数据列之间以间隔符为间隔无空格，行之间无空行
3. 注意:行之间无空行很重要，如果有空行或者数据集中行末有空格则会引发list index out of range

# 2. python处理csv文件的扩展库

## 2.1. 使用PythonI/O写入csv文件
```py
with open('file_name','w(r...)',encoding = 'GBK',newline = '') as f:
    writer = csv.writer(f)
    writer.writerows('content')
    f.close()
```

## 2.2. 使用PythonI/O读出csv文件
```py
import csv
with open('file_name','w(r...)',encoding = 'GBK(utf-8...)',newline = '') as f:
    datas = csv.reader(f)
    for data in datas:
        print(data)
```

## 2.3. 使用Pandas读取CSV文件
```py
import pandas as pd
csv_data = pd.read_csv('file_name')
print(csv_data.shape)
N = 5
csv_batch_data = csv_data.tail(N)# 取后5条数据
print(csv_batch_data.shape)
```

## 2.4. 使用Tensorflow读取CSV文件
```py
'''使用Tensorflow读取csv数据'''
filename = 'birth_weight.csv'
file_queue = tf.train.string_input_producer([filename])  # 设置文件名队列，这样做能够批量读取文件夹中的文件
reader = tf.TextLineReader(skip_header_lines=1)  # 使用tensorflow文本行阅读器，并且设置忽略第一行
key, value = reader.read(file_queue)
defaults = [[0.], [0.], [0.], [0.], [0.], [0.], [0.], [0.], [0.]]  # 设置列属性的数据格式
LOW, AGE, LWT, RACE, SMOKE, PTL, HT, UI, BWT = tf.decode_csv(value, defaults)
# 将读取的数据编码为我们设置的默认格式
vertor_example = tf.stack([AGE, LWT, RACE, SMOKE, PTL, HT, UI])  # 读取得到的中间7列属性为训练特征
vertor_label = tf.stack([BWT])  # 读取得到的BWT值表示训练标签
# 用于给取出的数据添加上batch_size维度，以批处理的方式读出数据。可以设置批处理数据大小，是否重复读取数据，容量大小，队列末尾大小，读取线程等属性。
example_batch, label_batch = tf.train.shuffle_batch([vertor_example, vertor_label], batch_size=10, capacity=100, min_after_dequeue=10)

# 初始化Session
with tf.Session() as sess:
    coord = tf.train.Coordinator()  # 线程管理器
    threads = tf.train.start_queue_runners(coord=coord)
    print(sess.run(tf.shape(example_batch)))  # [10  7]
    print(sess.run(tf.shape(label_batch)))  # [10  1]
    print(sess.run(example_batch)[3])  # [ 19.  91.   0.   1.   1.   0.   1.]
    coord.request_stop()
    coord.join(threads)

'''
对于使用所有Tensorflow的I/O操作来说开启和关闭线程管理器都是必要的操作
with tf.Session() as sess:
    coord = tf.train.Coordinator()  # 线程管理器
    threads = tf.train.start_queue_runners(coord=coord)
    #  Your code here~
    coord.request_stop()
    coord.join(threads)
'''
```


# 3. 处理csv文件中遇到的问题
1. 首先对于非数值类数据我们要首先确定它的编码方式，并且在open的过程中指定encoding的编码格式防止出现乱码。
2. 调整csv文件的编码格式:以下以使用excel为例，我们可以“文件”->“保存”->“Web选项”->“编码”
3. 参考:<a href = "https://jingyan.baidu.com/article/14bd256e8509a9bb6d261239.html">如何改变excel文件编码</a>