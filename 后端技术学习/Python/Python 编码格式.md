Python 编码格式的问题汇总
---

<!-- TOC -->

- [1. UnicodeEncodeError: 'gbk' codec can't encode character '\xa9' in position](#1-unicodeencodeerror-gbk-codec-cant-encode-character-xa9-in-position)
- [2. 爬取网页的编码问题的解决方案](#2-爬取网页的编码问题的解决方案)
  - [2.1. 方法一:对之后的对象进行解码(不推荐，有问题)](#21-方法一对之后的对象进行解码不推荐有问题)
  - [2.2. 方法二:一开始就处理网页编码形式](#22-方法二一开始就处理网页编码形式)

<!-- /TOC -->

# 1. UnicodeEncodeError: 'gbk' codec can't encode character '\xa9' in position 
```py
temp = string.encode('gbk','ignore')
print(temp.decode('gbk'))
```

1. 通过直接ignore来完成避免(较为通用)

# 2. 爬取网页的编码问题的解决方案

## 2.1. 方法一:对之后的对象进行解码(不推荐，有问题)
```python
import requests
from bs4 import BeautifulSoup
r = requests.get("https://www.biquke.com/bq/59/59567/8131142.html")
soup = BeautifulSoup(r.text,'lxml')
r.encoding()# ISO-8859-1需要转码成utf-8
pattern = soup.find_all('h1')
for x in pattern:
    x_temp = x.string
    x = x_temp.encode('ISO-8859-1')
    print("  "+x.decode('utf-8'))
```

## 2.2. 方法二:一开始就处理网页编码形式
```python
import requests
from bs4 import BeautifulSoup
r = requests.get("https://www.biquke.com/bq/59/59567/8131142.html")
r.encoding = 'utf-8'
soup = BeautifulSoup(r.text,'lxml')
r.encoding()# ISO-8859-1需要转码成utf-8
pattern = soup.find_all('h1')
for x in pattern:
    x_temp = x.string
    x = x_temp.encode('ISO-8859-1')
    print("  "+x.decode('utf-8'))
```
