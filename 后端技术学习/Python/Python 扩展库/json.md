Python中的json库
---
1. 主要是处理和生成非格式化数据

<!-- TOC -->

- [1. Python读取和处理json](#1-python读取和处理json)
- [2. Python保存为json字符串](#2-python保存为json字符串)

<!-- /TOC -->

# 1. Python读取和处理json
```py
import json
 
data1=json.loads(json_file)
print(data1) # {}
print(type(data1)) # dict
```

# 2. Python保存为json字符串
```py
import json
 
data={"name":"sunxiaomin","sex":"男","年龄":"26"}
#将python字典类型变成json数据格式
json_str=json.dumps(data)
print(json_str)
print(type(json_str)) # str
```