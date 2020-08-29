python Excel处理
---
1. 写入:安装xlwt库:`pip install xlwt`
2. 读取:安装xlrd库:`pip install xlrd`

<!-- TOC -->

- [1. 写入excel](#1-写入excel)
  - [1.1. 直接写入数据到excel中](#11-直接写入数据到excel中)
  - [1.2. 写入字典类型的数据](#12-写入字典类型的数据)
  - [1.3. 设置字体样式、单元格样式](#13-设置字体样式单元格样式)
    - [1.3.1. 初始化样式](#131-初始化样式)
    - [1.3.2. 设置字体大小、颜色](#132-设置字体大小颜色)
    - [1.3.3. 设置单元格边框线](#133-设置单元格边框线)
    - [1.3.4. 设置字体在单位格中的位置](#134-设置字体在单位格中的位置)
    - [1.3.5. 设置单元格背景颜色](#135-设置单元格背景颜色)
    - [1.3.6. 设置之后进行重新的赋值](#136-设置之后进行重新的赋值)
  - [1.4. write()写入excel](#14-write写入excel)
- [2. 读取Excel](#2-读取excel)
  - [2.1. 常见函数](#21-常见函数)
    - [2.1.1. 获取一个工作表](#211-获取一个工作表)
    - [2.1.2. 行的操作](#212-行的操作)
    - [2.1.3. 列的操作](#213-列的操作)
    - [2.1.4. 单元格的操作](#214-单元格的操作)
- [3. 参考](#3-参考)

<!-- /TOC -->

# 1. 写入excel

## 1.1. 直接写入数据到excel中
```py
def write_file():
    book = xlwt.Workbook(encoding='utf-8') #创建Workbook，相当于创建Excel
    # 创建sheet，Sheet1为表的名字，cell_overwrite_ok为是否覆盖单元格
    sheet1 = book.add_sheet(u'Sheet1', cell_overwrite_ok=True)
    #向表中添加数据
    sheet1.write(0, 0, 'Englishname')  #第0行第0列
    sheet1.write(1, 0, 'Hellen')  #第一行第0列
    sheet1.write(0, 1, '中文名字')
    sheet1.write(1, 1, '海伦')
```

## 1.2. 写入字典类型的数据
```py
def write_file():
    book = xlwt.Workbook(encoding='utf-8') #创建Workbook，相当于创建Excel
    # 创建sheet，Sheet1为表的名字，cell_overwrite_ok为是否覆盖单元格
    sheet1 = book.add_sheet(u'Sheet1', cell_overwrite_ok=True)
    data = {
            "序号": ["姓名", "语文", "数学", "英语"],
            "1": ["张三", 130, 120, 100],
            "2": ["李四", 100, 110, 120],
            "3": ["王五", 125, 135, 135]
           }
    r = 0
    for i, j in data.items():   # i表示data中的key，j表示data中的value
        le = len(j)   # values返回的列表长度
        if r == 0:
            sheet1.write(r, 0, i, set_style('Arial', 220, True)) #添加第 0 行 0 列数据单元格背景设为黄色
        else:
            sheet1.write(r, 0, i, )  # 添加第 1 列的数据
        for c in range(1, le + 1):  #values列表中索引
            if r == 0:
                sheet1.write(r, c, j[c - 1], set_style('Arial', 220, True))  #添加第 0 行，2 列到第 5 列的数据单元格背景设为黄色
            else:
                sheet1.write(r, c, j[c - 1])
        r += 1  # 行数
    #sheet_merge() 合并单元格
    book.save('D:\\write.xlsx')# 存储单元格
```

## 1.3. 设置字体样式、单元格样式

### 1.3.1. 初始化样式
`style = xlwt.XFStyle()   #初始化样式`

### 1.3.2. 设置字体大小、颜色
```py
font = xlwt.Font()   #创建字体, Font定义字体的大小、颜色
font.name = name    #字体名称
font.bold = bold   #粗体
# font.italic = True  #斜体
font.height = height  #字体大小
# font.colour_index = 4  # 字体颜色
```

### 1.3.3. 设置单元格边框线
```py
borders = xlwt.Borders()  #单元格边框线
borders.left = xlwt.Borders.THIN   #设置边框线粗细
borders.right = xlwt.Borders.THIN
borders.top = xlwt.Borders.THIN
borders.bottom = xlwt.Borders.THIN
# borders.right_colour = 1  #设置边框线颜色
# borders.left_colour = 1
# borders.top_colour = 1
# borders.bottom_colour = 1
```

### 1.3.4. 设置字体在单位格中的位置
```py
alignment = xlwt.Alignment() #设置字体在单元格中的位置
alignment.horz =  xlwt.Alignment.HORZ_CENTER  #水平居左
alignment.vert = xlwt.Alignment.VERT_CENTER  #垂直居中
```

### 1.3.5. 设置单元格背景颜色
```py
pat = xlwt.Pattern()  #设置单元格背景颜色
pat.pattern = xlwt.Pattern.SOLID_PATTERN
pat.pattern_fore_colour = 5   #黄色
```

### 1.3.6. 设置之后进行重新的赋值
```py
style.font = font
style.borders = borders
style.alignment = alignment
style.pattern = pat
```

## 1.4. write()写入excel
```py
sheet1.write(r, 0, i, set_style('Arial', 220, True)) #添加第 0 行 0 列数据单元格背景设为黄色
```

# 2. 读取Excel
1. 导入模块:`import xlrd`
2. 打开Excel数据流:`data = xlrd.open_workbook(filename)#文件名以及路径，如果路径或者文件名有中文给前面加一个r拜师原生字符。`

## 2.1. 常见函数

### 2.1.1. 获取一个工作表
```py
table = data.sheets()[0]          #通过索引顺序获取
table = data.sheet_by_index(sheet_indx) #通过索引顺序获取
table = data.sheet_by_name(sheet_name)#通过名称获取
# 以上三个函数都会返回一个xlrd.sheet.Sheet()对象
names = data.sheet_names()    #返回book中所有工作表的名字
data.sheet_loaded(sheet_name or indx)   # 检查某个sheet是否导入完毕
```
### 2.1.2. 行的操作
```py
nrows = table.nrows  #获取该sheet中的有效行数
table.row(rowx)  #返回由该行中所有的单元格对象组成的列表
table.row_slice(rowx)  #返回由该列中所有的单元格对象组成的列表
table.row_types(rowx, start_colx=0, end_colx=None)    #返回由该行中所有单元格的数据类型组成的列表
table.row_values(rowx, start_colx=0, end_colx=None)   #返回由该行中所有单元格的数据组成的列表
table.row_len(rowx) #返回该列的有效单元格长度
```
### 2.1.3. 列的操作
```py
ncols = table.ncols   #获取列表的有效列数
table.col(colx, start_rowx=0, end_rowx=None)  #返回由该列中所有的单元格对象组成的列表
table.col_slice(colx, start_rowx=0, end_rowx=None)  #返回由该列中所有的单元格对象组成的列表
table.col_types(colx, start_rowx=0, end_rowx=None)    #返回由该列中所有单元格的数据类型组成的列表
table.col_values(colx, start_rowx=0, end_rowx=None)   #返回由该列中所有单元格的数据组成的列表
```

### 2.1.4. 单元格的操作
```py
table.cell(rowx,colx)   #返回单元格对象
table.cell_type(rowx,colx)    #返回单元格中的数据类型
table.cell_value(rowx,colx)   #返回单元格中的数据
table.cell_xf_index(rowx, colx)   # 暂时还没有搞懂
```

# 3. 参考
1. <a href = "https://www.cnblogs.com/insane-Mr-Li/p/9092619.html">python里面的xlrd模块详解（一）</a>