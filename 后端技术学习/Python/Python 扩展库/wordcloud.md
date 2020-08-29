wordcloud
---
1. 本文主要记录了如何绘制词云的简单过程。

<!-- TOC -->

- [1. 准备工作](#1-准备工作)
- [2. 具体编码](#2-具体编码)
- [不支持中文的解决方案](#不支持中文的解决方案)
- [3. 参考文献](#3-参考文献)

<!-- /TOC -->

# 1. 准备工作
```
以下是需要的扩展库
re
collections
numpy
jieba
wordcloud
PIL
matplotlib.pyplot
```

1. 我们打算统计的文件，命名为:`article.txt`
2. 我们打算使用做为背景的图片:`wordcloud.jpg`

# 2. 具体编码
```py
# 导入扩展库
import re # 正则表达式库
import collections # 词频统计库
import numpy as np # numpy数据处理库
import jieba # 结巴分词
import wordcloud # 词云展示库
from PIL import Image # 图像处理库
import matplotlib.pyplot as plt # 图像展示库

# 读取文件
fn = open('article.txt') # 打开文件
string_data = fn.read() # 读出整个文件
fn.close() # 关闭文件

# 文本预处理
pattern = re.compile(u'\t|\n|\.|-|:|;|\)|\(|\?|"') # 定义正则表达式匹配模式
string_data = re.sub(pattern, '', string_data) # 将符合模式的字符去除

# 文本分词
seg_list_exact = jieba.cut(string_data, cut_all = False) # 精确模式分词
object_list = []

# 可以替换为从文件读入停用词
remove_words = [u'的', u'，',u'和', u'是', u'随着', u'对于', u'对',u'等',u'能',u'都',u'。',u' ',u'、',u'中',u'在',u'了',u'通常',u'如果',u'我们',u'需要'] # 自定义去除词库

for word in seg_list_exact: # 循环读出每个分词
    if word not in remove_words: # 如果不在去除词库中
        object_list.append(word) # 分词追加到列表

# 词频统计
word_counts = collections.Counter(object_list) # 对分词做词频统计
word_counts_top10 = word_counts.most_common(10) # 获取前10最高频的词
print (word_counts_top10) # 输出检查

# 词频展示
mask = np.array(Image.open('wordcloud.jpg')) # 定义词频背景
wc = wordcloud.WordCloud(
    font_path = 'C:/Windows/Fonts/simhei.ttf', # 设置字体格式
    mask = mask, # 设置背景图
    max_words = 200, # 最多显示词数
    max_font_size = 100 # 字体最大值
)

wc.generate_from_frequencies(word_counts) # 从字典生成词云
image_colors = wordcloud.ImageColorGenerator(mask) # 从背景图建立颜色方案
wc.recolor(color_func = image_colors) # 将词云颜色设置为背景图方案
plt.imshow(wc) # 显示词云
plt.axis('off') # 关闭坐标轴
plt.show() # 显示图像
```

# 不支持中文的解决方案
1. 我们还需要将使用jieba进行分词，然后字体需要设置ttf文件

# 3. 参考文献
1. <a href = "https://www.jianshu.com/p/28718ba04bc9?from=groupmessage">Python实现一个词频统计(词云)图</a>