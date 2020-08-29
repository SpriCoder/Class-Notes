<!-- TOC -->

- [1. os](#1-os)
- [2. 查看文件文件夹属性](#2-查看文件文件夹属性)
  - [2.1. 当前路径以及路径下的文件](#21-当前路径以及路径下的文件)
  - [2.2. 获得绝对路径](#22-获得绝对路径)
  - [2.3. 查看路径的文件夹部分和文件名部分](#23-查看路径的文件夹部分和文件名部分)
  - [2.4. 查看文件时间](#24-查看文件时间)
  - [2.5. 查看文件大小](#25-查看文件大小)
  - [2.6. 查看文件是否存在](#26-查看文件是否存在)
  - [2.7. 查看表示参数](#27-查看表示参数)
- [3. 修改文件文件夹](#3-修改文件文件夹)
  - [3.1. 文件夹重命名](#31-文件夹重命名)
  - [3.2. 创建目录](#32-创建目录)
  - [3.3. 清空文件夹](#33-清空文件夹)
- [4. 查看文件和文件夹类型](#4-查看文件和文件夹类型)
  - [4.1. isfile(path)函数](#41-isfilepath函数)
  - [4.2. isdir(path)](#42-isdirpath)
- [5. 参考](#5-参考)

<!-- /TOC -->

# 1. os
1. 在自动化测试中，经常需要中查找操作文件，比如说查找配置文件(从而读取配置文件的信息)，查找测试报告，经常要对大量文件和大量路径进行操作，这就依赖于os模块

# 2. 查看文件文件夹属性

## 2.1. 当前路径以及路径下的文件
1. `os.getcwd()`:查看当前所在路径
2. `os.listdir(path)`:列举目录下的所有文件，**返回的是列表类型**
```py
>>> import os
>>> os.getcwd()
'D:\\pythontest\\ostest'
>>> os.listdir(os.getcwd())
['hello.py', 'test.txt']
```

## 2.2. 获得绝对路径
1. `os.path.abspath(path)`
```py
>>> os.path.abspath('.')
'D:\\pythontest\\ostest'
>>> os.path.abspath('..')
'D:\\pythontest'
os.path.abspath('...')
'D:\\pythontest\\ostest\\'
```

## 2.3. 查看路径的文件夹部分和文件名部分
1. `os.path.split(path)`:将路径分解为(文件夹，文件名)，返回的是**元组**类型。
    + 可以看出，若路径字符串最后一个字符是\,则只有文件夹部分有值；若路径字符串中均无\,则只有文件名部分有值。若路径字符串有\，且不在最后，则文件夹和文件名均有值。且返回的文件夹的结果不包含\.
2. `os.path.join(path1,path2,...)`:将path进行组合，如果其中有绝对路径，则之前的path将被删除。
```py
>>> os.path.split('D:\\pythontest\\ostest\\Hello.py')
('D:\\pythontest\\ostest', 'Hello.py')
>>> os.path.split('.')
('', '.')
>>> os.path.split('D:\\pythontest\\ostest\\')
('D:\\pythontest\\ostest', '')
>>> os.path.split('D:\\pythontest\\ostest')
('D:\\pythontest', 'ostest')
>>> os.path.join('D:\\pythontest', 'ostest')
'D:\\pythontest\\ostest'
>>> os.path.join('D:\\pythontest\\ostest', 'hello.py')
'D:\\pythontest\\ostest\\hello.py'
>>> os.path.join('D:\\pythontest\\b', 'D:\\pythontest\\a')
'D:\\pythontest\\a'
```
3. `os.path.dirname(path)`:返回path中的文件夹部分，结果不包含`\`
```py
>>> os.path.dirname('D:\\pythontest\\ostest\\hello.py')
'D:\\pythontest\\ostest'
>>> os.path.dirname('.')
''
>>> os.path.dirname('D:\\pythontest\\ostest\\')
'D:\\pythontest\\ostest'
>>> os.path.dirname('D:\\pythontest\\ostest')
'D:\\pythontest'
```
4. `os.path.basename(path)`:返回path中的文件名
```py
>>> os.path.basename('D:\\pythontest\\ostest\\hello.py')
'hello.py'
>>> os.path.basename('.')
'.'
>>> os.path.basename('D:\\pythontest\\ostest\\')
''
>>> os.path.basename('D:\\pythontest\\ostest')
'ostest'
```

## 2.4. 查看文件时间
1. `os.path.getmtime(path):`文件或文件夹的最后修改时间，从新纪元到访问时的秒数。
2. `os.path.getatime(path):`文件或文件夹的最后访问时间，从新纪元到访问时的秒数。
3. `os.path.getctime(path):`文件或文件夹的创建时间，从新纪元到访问时的秒数。

## 2.5. 查看文件大小
1. `os.path.getsize(path)`:文件或文件夹的大小，若是文件夹返回0

## 2.6. 查看文件是否存在
1. `os.path.exists(path)`:文件或文件夹是否存在，返回True或False

## 2.7. 查看表示参数
1. `os.sep`=> `\\`
2. `os.extsep`=>`.`
3. `os.pathsep`=>`;`
4. `os.linesep`=>`\r\n`

# 3. 修改文件文件夹

## 3.1. 文件夹重命名
```py
os.rename("oldname","newname")#重命名文件/目录
os.stat('path/filename')#获取文件/目录信息
```
1. stat结构说明
    1. st_mode: inode 保护模式
    2. st_ino: inode 节点号。
    3. st_dev: inode 驻留的设备。
    4. st_nlink: inode 的链接数。
    5. st_uid:  所有者的用户ID。
    6. st_gid:  所有者的组ID。
    7. st_size: 普通文件以字节为单位的大小；包含等待某些特殊文件的数据。
    8. st_atime: 上次访问的时间。
    9. st_mtime: 最后一次修改的时间。
    10. st_ctime: 由操作系统报告的"ctime"。在某些系统上（如Unix）是最新的元数据更改的时间，在其它系统上（如Windows）是创建时间（详细信息参见平台的文档）。
 

## 3.2. 创建目录
```py
os.makedirs('dir1/dir2') #可生成多层递归目录
os.mkdir('dir3')#生成单级目录；相当于shell中mkdir dirname
```

## 3.3. 清空文件夹
```py
os.rmdir('dir3/dir4')　　　   # 删除单级空目录，若目录不为空则无法删除，报错；相当于shell中rmdir dirname
os.removedirs('dir3/dir4')　　#若目录为空，则删除，并递归到上一级目录，如若也为空，则删除，依此类推
```

# 4. 查看文件和文件夹类型

## 4.1. isfile(path)函数
1. isfile(path)是用来查探当前路径是否是一个文件
```py
os.path.isfile('c:\\boot.ini')
True
os.path.isfile('c:\\csv\\test.csv')
False
os.path.isfile('c:\\csv\\')
False
```

## 4.2. isdir(path)
1. isdir(path)是用来查探当前路径是否是一个目录


# 5. 参考
1. <a href = "https://www.cnblogs.com/yufeihlf/p/6179547.html">os</a>
2. <a href = "https://blog.csdn.net/qq_20412595/article/details/82990515">Python OS 文件/目录方法</a>