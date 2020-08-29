zipfile
---
1. Python的用于解压压缩包的库

<!-- TOC -->

- [1. 打开zip文件](#1-打开zip文件)
- [2. 查看压缩包中的文件名问题](#2-查看压缩包中的文件名问题)
- [3. 压缩包文件中文乱码问题](#3-压缩包文件中文乱码问题)
- [4. 查看压缩包中的文件原始大小和压缩后大小](#4-查看压缩包中的文件原始大小和压缩后大小)
- [5. 文件解压缩](#5-文件解压缩)
  - [5.1. 特定文件解压缩](#51-特定文件解压缩)
  - [5.2. 全部文件解压缩](#52-全部文件解压缩)
- [6. 压缩文件](#6-压缩文件)
  - [6.1. 多个文件压缩](#61-多个文件压缩)
  - [6.2. 向已有压缩包添加文件](#62-向已有压缩包添加文件)
- [7. 参考](#7-参考)

<!-- /TOC -->

# 1. 打开zip文件
```py
import zipfile
with zipfile.ZipFile('demo.zip','r') as zzz:
    print("Success")
```

# 2. 查看压缩包中的文件名问题
```py
with zipfile.ZipFile('demo.zip','r') as zzz:
    print(zzz.namelist())
# 运行结果：['file1.txt', 'file2.txt', 'temp_file.png']
```

# 3. 压缩包文件中文乱码问题
```py
with zipfile.ZipFile('demo.zip','r') as zzz:
    for f_name in zzz.namelist():
        print(f_name.encode('cp437').decode('gbk'))
''' 运行结果：
file1.txt
file2.txt
temp_file.png
数据分析.PNG'''
```

# 4. 查看压缩包中的文件原始大小和压缩后大小
```py
with zipfile.ZipFile('demo.zip','r') as zzz:
    for f_name in zzz.namelist():
        f_info = zzz.getinfo(f_name)
        f_name = f_name.encode('cp437').decode('gbk')
        # 输出该文件的文件大小.file_size和压缩之后的大小compress_size
        print(f_name,f_info.file_size,f_info.compress_size)
''' 运行结果：
file1.txt 98 47
file2.txt 98 47
temp_file.png 22061 18211
数据分析.PNG 178210 177931'''
# 除了大小之外.getinfo()还可以读取出filename、compress_type、external_attr属性。
```

# 5. 文件解压缩

## 5.1. 特定文件解压缩
```py
with zipfile.ZipFile('demo.zip','r') as zzz:
    zzz.extract('file1.txt',target_place)
# 把压缩包里的file1.txt解压到当前目录
# 如果压缩包里出现中文，则需要进行转码（所以在用Python压缩文件和解压的时候最好不要出现中文路径和文件名）
```

## 5.2. 全部文件解压缩
```py
with zipfile.ZipFile('Python.zip','r') as zzz:
    zzz.extractall('解压到这里',pwd=b'123456')
```
1. 这里表示可以使用计算机暴力破解压缩包密码

# 6. 压缩文件

## 6.1. 多个文件压缩
```py
f_list = ['file1.txt','file2.txt','temp_file.png','./py/hello.py']
with zipfile.ZipFile('sample.zip','w') as z:
    for f_name in f_list:
        z.write(f_name)
```

## 6.2. 向已有压缩包添加文件
```py

f_list = ['temp_folder.png','./py/game.py']
with zipfile.ZipFile('sample.zip','a') as z:
    for f_name in f_list:
        z.write(f_name)
```

1. `w`的方式表示是覆盖写
2. `a`的方式表示是添加写

# 7. 参考
1. <a href = "https://www.icoa.cn/a/883.html">Python读取、创建和压缩ZIP</a>