pandas-profiling(数据预览分析)
---

# 1. 安装pandas-profiling安装与调用
```py
pip install pandas-profiling
import pandas as pd
import pandas_profiling
```

# 2. 数据预览分析过程

## 2.1. 导入数据
`data=pd.read_csv("model.csv")`

## 2.2. 直接查看
`pandas_profiling.ProfileReport(data)`

# 3. 数据概述

## 3.1. 总体数据

## 3.2. 警告信息

## 3.3. 单变量描述

## 3.4. 相关性分析

## 3.5. 生产HTML报告文件

# 4. 参考
1. <a href = "https://blog.csdn.net/Andy_shenzl/article/details/81709409">pandas-profiling</a>