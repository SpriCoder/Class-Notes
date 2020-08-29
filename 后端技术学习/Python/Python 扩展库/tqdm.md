<!-- TOC -->

- [1. tqdm](#1-tqdm)
- [2. 使用方法](#2-使用方法)

<!-- /TOC -->

# 1. tqdm
1. Tqdm是一个快速，可扩展的Python进度条，可以在Python长循环中添加一个进度显示信息，用户需要封装任意的迭代器tqdm的迭代器。

# 2. 使用方法
1. tqdm(list)方法可以传入任意一种list，比如数组。或者string的数组。
```python
from tqdm import tqdm
for i in tqdm(range(1000)):  
     #do something
     pass  
```
2. 使用trange:这个是tqdm(range(i))的简单写法
```python
from tqdm import trange
for i in trange(100):
    #do something
    pass
```
3. 手动方法:使用for循环外部初始化tqdm，可以打印其他信息。
```python
pbar = tqdm(["a", "b", "c", "d"])
for char in pbar:
    pbar.set_description("Processing %s" % char)
```