wget
---

<!-- TOC -->

- [1. 什么wget呢？](#1-什么wget呢)
- [2. 安装wget](#2-安装wget)
- [3. 函数](#3-函数)

<!-- /TOC -->

# 1. 什么wget呢？
1. wget是一个网络上自动下载文件的自由工具。它支持HTTP，HTTPS和FTP协议，可以使用HTTP代理

# 2. 安装wget
1. `pip install wget`

# 3. 函数
```py
# -*- coding: utf-8 -*-
import wget, tarfileimport os # 网络地址
DATA_URL = 'http://www.robots.ox.ac.uk/~ankush/data.tar.gz'# 本地硬盘文件# 
DATA_URL = '/home/xxx/book/data.tar.gz' out_fname = 'abc.tar.gz' wget.download(DATA_URL, out=out_fname)# 提取压缩包
tar = tarfile.open(out_fname)
tar.extractall()
tar.close()# 删除下载文件
os.remove(out_fname)
```