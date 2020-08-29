<!-- TOC -->

- [1. Python自然语言处理工具包NLTK](#1-python自然语言处理工具包nltk)
  - [1.1. 古腾堡语料库](#11-古腾堡语料库)
  - [1.2. 函数](#12-函数)
  - [1.3. 本地文本载入](#13-本地文本载入)
- [2. 使用NLTK进行情感分析](#2-使用nltk进行情感分析)

<!-- /TOC -->

# 1. Python自然语言处理工具包NLTK
1. 包含古腾堡语料库等
2. 进行处理
 
## 1.1. 古腾堡语料库
1. 导入:`import nltk`
2. 下载语料库:`nltk.download()`
3. 进入下载器进行下载
4. 语料库下载到本地
5. 加载语料库:`from nltk.corpus import gutenberg`
6. 查看相应语料库的收录什么相应的书籍:`gutenburg.fileids()`
7. 列出书中的函数:`gutenburg.words()`

## 1.2. 函数
1. `.word('name.txt')`统计单词和标点的个数，返回的是一个列表
2. `.count(str)`:返回其出现次数
3. `FreqDist()`:创建一个所给数据的频率分布
4. `.B()`不同单词个数
5. `.N()`所有单词个数
6. `.tabulate(number)`将前number个数据按照表格显示出来
7. `fd3.freq(str)`计算出现频率
8. `ConditionalFreqDist()`条件频率统计函数
    + 研究类别间的系统差异
9. 去除停用词(the)

## 1.3. 本地文本载入
1. 本地文本语料库的载入只要使用NLTK中的**PlaintextCorpusReader()**函数。
2. 假设文件在D盘的data目录下，只要将函数参数corpus_root的值设置为这个目录，函数第二个参数可以用一个文件列表例如['name.txt', 'new/first.txt']，表示载入这个文件列表中的相应文件，也可以利用正则表达式指定要载入的文件，例如用'.*'表示目录下的所有文件。例如：
```python
>>> from nltk.corpus import PlaintextCorpusReader
>>> corpus_root = r'd:\data'
>>> books = PlaintextCorpusReader(corpus_root, '.*')
>>> books.fileids()
['test/万历十五年.txt', '三国演义.txt', '水浒传.txt', '红楼梦.txt', '西游记.txt']
```

# 2. 使用NLTK进行情感分析
1. 如果提示确实某些包，则按照报错提示修正即可。