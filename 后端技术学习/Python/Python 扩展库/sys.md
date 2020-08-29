sys
---
1. 该模块提供对解释器使用或维护的一些变量的访问，以及与解释器进行交互的函数。
2. 以下简要介绍了sys的部分方法参数

<!-- TOC -->

- [1. sys.argv](#1-sysargv)
- [2. sys.byteorder](#2-sysbyteorder)
- [3. sys.builtin_module_names](#3-sysbuiltin_module_names)
- [4. 一个样例](#4-一个样例)
- [5. 参考](#5-参考)

<!-- /TOC -->
# 1. sys.argv
1. 传递给Python脚本的命令行参数列表。
2. argv[0]是脚本名称(依赖于OS，无论是否是完整的路径)
3. 之后argv[]链表中为顺序向下排序的参数

# 2. sys.byteorder
1. 本机自己顺序的指示符
2. big
3. big-endian：最重要的字符优先
4. little
5. little-endian：最不重要的字符优先

# 3. sys.builtin_module_names
1. 一个字符串元组

# 4. 一个样例
```python
#-*- coding:utf-8 -*-
import os
import io
from surprise import KNNBaseline
from surprise import Dataset
from surprise import Reader
import logging
import sys

def getSimModle(file_path):
    reader = Reader(line_format="user item rating", sep ="|")  # 分隔符为空白键
    data = Dataset.load_from_file(file_path, reader=reader)
    trainset = data.build_full_trainset()
    sim_options = {"name":'pearson_baseline','usr_based':False}
    algo = KNNBaseline(sim_options=sim_options)
    algo.fit(trainset)
    return algo

def read_item_names(file_name):
    type_to_uid = {}
    uid_to_type = {}
    with io.open(file_name,'r',encoding = 'ISO-8859-1') as f:
        for line in f:
            line = line.split("|")
            uid_to_type[line[0]] = line[1]
            type_to_uid[line[1]] = line[0]
    return uid_to_type, type_to_uid

def showSimilarMovies(algo, uid_to_type, type_to_uid):
    print(type_to_uid)
    type1_uid = type_to_uid["1"]
    logging.debug('raw_id='+type1_uid)

    type1_inner_uid = algo.trainset.to_inner_iid(type1_uid)
    logging.debug('inner_id = '+str(type1_inner_uid))

    type1_neighbors = algo.get_neighbors(type1_inner_uid,2)
    logging.debug("neighbors_id = " + str(type1_neighbors))

    neighbors_raw_ids = [algo.trainset.to_raw_iid(inner_id) for inner_id in type1_neighbors]
    neighbors_movies = [uid_to_type[raw_id ] for raw_id in neighbors_raw_ids]
    print('The 2 nearest_neighbors of 1 are:')
    for movie in neighbors_movies:
        print(movie)

if __name__ == '__main__':
    file_name = sys.argv[1];#"D:/basic/data.txt"
    uid_to_type,type_to_uid = read_item_names(file_name)

    algo = getSimModle(file_name)

    showSimilarMovies(algo,uid_to_type,type_to_uid)
```

# 5. 参考
1. <a href = "https://blog.csdn.net/qq_38526635/article/details/81739321">sys模块简介</a>

