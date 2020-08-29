python处理word文档
---

<!-- TOC -->

- [1. 标准库](#1-标准库)
- [2. 使用标准库的基本步骤](#2-使用标准库的基本步骤)
  - [2.1. 不同的步骤的相应源码](#21-不同的步骤的相应源码)
    - [2.1.1. 第一步：创建文档对象](#211-第一步创建文档对象)
    - [2.1.2. 第二步：设置文档格式](#212-第二步设置文档格式)
    - [2.1.3. 第三步：添加相应数据](#213-第三步添加相应数据)
      - [2.1.3.1. 添加段落文本](#2131-添加段落文本)
      - [2.1.3.2. 添加表格文本](#2132-添加表格文本)
      - [2.1.3.3. 添加图像](#2133-添加图像)
    - [2.1.4. 第四步：保存doc对象](#214-第四步保存doc对象)
- [3. 使用这个库的注意事项](#3-使用这个库的注意事项)
- [4. 参考文献](#4-参考文献)

<!-- /TOC -->

# 1. 标准库
1. 安装`pip install python-docx`库

# 2. 使用标准库的基本步骤
1. 建立一个文档对象(可自动使用默认模板建立，也可以使用已有文件)。
2. 设置文档的格式(默认字体、页面边距等)。
3. 在文档对象中加入段落文本、表格、图像等，并指定其样式。
4. 保存文档。

## 2.1. 不同的步骤的相应源码

### 2.1.1. 第一步：创建文档对象
```python
from docx import document
doc = Document()
doc = Document("file_name")
```
### 2.1.2. 第二步：设置文档格式
```python
from docx.shared import Inches,Pt

def chg_font(obj,fontname = '仿宋',size = None):
    ## 设置字体函数
    obj.font.name = fontname
    obj._element.rPr.rFonts.set(qn('w:eastAsia'),fontname)
    if size and isinstance(size,Pt):
        obj.font.size = size

distance = Inches(0.3)

sec = doc.sections[0]# sections对应文档中的“节”
sec.left_margin = distance # 以下依次设置左、右、上、下页面边距
sec.right_margin = distance
sec.top_margin = distance sec.bottom_margin = distance sec.page_width =Inches(12)    #设置页面宽度
sec.page_height = Inches(20)  #设置页面高度 
##设置默认字体
chg_font(doc.styles['Normal'],fontname='宋体')
```

### 2.1.3. 第三步：添加相应数据

#### 2.1.3.1. 添加段落文本
```python
paragraph =doc.add_paragraph('text....') ph_format =paragraph.paragraph_format ph_format.space_before =Pt(10)#设置段前间距
ph_format.space_after =Pt(12)#设置段后间距
ph_format.line_spacing=Pt(19)#设置行间距
```
2. 如果同一段落中的文本格式不同，就需要使用Run对象(可以被理解成为单位设置格式的段落内对象)
```python
run = paragraph.add_run('text...')
run.bold = True #设置字体为粗体
chg_font(run,fontname='微软雅黑', size=Pt(12))  #设置字体和字号
```
#### 2.1.3.2. 添加表格文本
1. 创建表格对象
```python
tab =doc.add_table(rows=4,cols=4)   #添加一个4行4列的空表
cell=tab.cell(1,3)  #获取某单元格对象（从0开始索引）
```
2. 向单元格中添加单行文本
```python
cell.text = "abc" #向单元格添加单行文本
```
3. 在单元格中添加多行文本
```python
ph =cell.paragraphs[0]
run=ph.add_run(‘text....’)
run.add_break()# 添加一个折行
```
#### 2.1.3.3. 添加图像
```python
run.add_picture('a.png')# 插入图像，可以是内存中的图像，width=Inches(1.0)指定宽度。
```

### 2.1.4. 第四步：保存doc对象
```python
doc.save('a.doc')
```


# 3. 使用这个库的注意事项
1. 本库只支持Word2007以后版本的文档类型，即扩展名为.docx的。

# 4. 参考文献
1. <a href = "https://blog.csdn.net/cloveses/article/details/81668797">参考CSDN博客</a>
2. <a href = "https://blog.csdn.net/sunchengquan/article/details/80369304">其他详情细节</a>