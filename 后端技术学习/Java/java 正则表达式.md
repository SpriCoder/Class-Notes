<!-- TOC -->

- [1. 正则表达式](#1-正则表达式)
  - [1.1. java正则表达式](#11-java正则表达式)
  - [1.2. java.util.regex包](#12-javautilregex包)
  - [1.3. 捕获组](#13-捕获组)
  - [1.4. 正则表达式语法](#14-正则表达式语法)
- [2. Pattern类](#2-pattern类)
  - [2.1. 各种方法详解](#21-各种方法详解)
  - [2.2. 详见](#22-详见)
- [3. Matcher](#3-matcher)
  - [3.1. 索引方法](#31-索引方法)
  - [3.2. 研究方法](#32-研究方法)
  - [3.3. 替换方法](#33-替换方法)
  - [3.4. 返回指定位置](#34-返回指定位置)
- [4. 正则表达式注意](#4-正则表达式注意)
- [5. 扩展阅读](#5-扩展阅读)

<!-- /TOC -->
# 1. 正则表达式
1. 正则表达式定义了字符串的模式
2. 正则表达式用于搜索、编辑或处理文本
3. 在不同语言中有细微的差别

## 1.1. java正则表达式

符号|匹配
-|-
`\s+`|匹配一个字符或者多个字符
`\d+`|匹配一个或多个数字
`\`|下一字符标记为特殊字符、文本、反向应用或八进制转义符
`^`|匹配输入字符串的头部位置
`$`|匹配输入字符串的结尾位置
`*`|零次或者多次匹配前面的字符或子表达式
`+`|一次或多次匹配前面的字符或子表达式
`?`|零次或者一次匹配前面的字符或子表达式
`{n}`|n是非负整数。正好匹配n次
`{n,}`|n是非负整数。至少匹配n次
`{n,m}`|n，m是非负整数。至少匹配n次，至多m次
`?`|匹配方式可以被视作是"非贪心的"
`.`|匹配除了\r\n之外的任何单个字符
`(pattern)`|匹配pattern并捕获该匹配的子表达式
`(?:pattern)`|匹配pattern但不捕获该比配的子表达式
`(?=pattern)`|执行正向预测先行搜索
`(?!pattern)`|执行反向预测先行搜索
`x|y`|匹配x或y
`[xyz]`|字符集，匹配包含的任一字符
`[^xyz]`|反向字符集，匹配未被包含的一切字符
`[a-z]`|字符范围。匹配指定范围内的任何字符
`[^a-z]`|反向字符范围。匹配指定范围外的任何字符
`\b`|匹配一个字边界，er\b不匹配her
`\B`|非字边界匹配
`\cx`|匹配x指示的控制字符,\cM匹配Control-M或者回车符
`\d`|匹配数字字符
`\D`|匹配非数字字符
`\f`|换页符匹配
`\n`|换行符匹配
`\r`|匹配一个回车符
`\s`|匹配任何空白字符
`\S`|匹配任何非空白字符
`\t`|匹配制表符
`\v`|匹配垂直制表符
`\w`|匹配任何字类字符，包括下划线
`\W`|与任何非单词字符匹配
`\xn`|匹配十六进制，n为两数长的十六进制转义码
`\num`|匹配num,此处num是一个正整数，匹配多次(.)\1匹配两个相同字符
`\n`|匹配八进制转义码或反向引用
`\nm`|表示一个八进制转义码或反向引用

## 1.2. java.util.regex包
1. Pattern
    + 对象是一个正则表达式的编译表示
    + 静态创立
2. Matcher
    + 对输入字符串进行解释和匹配操作的引擎
    + 调用Pattern对象的matcher方法获得Matcher对象
3. PatternSyntaxException
4. 例子：`Pattern.matches(pattern,content)`

## 1.3. 捕获组
1. 把多个字符当一个单独单元进行处理，通过对括号内的字符分组来创建
2. 使用matcher方法中的groupCount方法来查看有多少分组  
3. 通过Matcher对象的group方法来按照组进行捕获
    1. group(0)是捕获整个组
    2. 之后按照1基的数字进行对于整体的捕获，也就是对字符串切割
```java 
String line = "a3000b";
String pattern = "(\\D*)(\\d+)(.*)";

Pattern r = Pattern.compile(pattern);

Matcher m = r.matcher(line);\\之后对m使用group方法即可
```

## 1.4. 正则表达式语法
1. `\\`:我要插入一个正则表达式的反斜线，所以其后的字符具有特殊的意义。
    + `\\\\`表示一个普通的反斜杠
2. 参加上面表格

# 2. Pattern类
1. Pattern类定义:`public final class Pattern extends Object implementsSerializable`
    + 是正则表达式的编译表示形式。
    + 主要任务是在编译正则表达式之后创建一个匹配模式。

## 2.1. 各种方法详解
方法|功能
`Pattern complie(String regex)`|编译正则表达式，并创建Pattern类(简单工厂方法)
`String pattern()`|返回正则表达式的字符串的形式
`Pattern compile(String regex, int flags)`|添加flag用来控制正则表达式的匹配行为
`int flags()`|返回当前Pattern的匹配flag参数
`String[] split(CharSequence input)`|用来分隔字符串
`static boolean matches(String regex, CharSequenceinput)`|静态方法，用于快速匹配字符串，仅适用一次，且匹配全部字符串。
`Matcher matcher(CharSequence input)`|返回一个matcher对象

## 2.2. 详见
<a href = "https://blog.csdn.net/zengxiantao1994/article/details/77803960">Java Pattern和Matcher的字符串详解</a>

# 3. Matcher

## 3.1. 索引方法
方法|作用
--|--
`public int start()`|返回以前匹配的初识索引
`public int start(int group)`|返回在以前的匹配操作期间，由给定组所捕获的子序列的初识索引
`public int end()`|返回最后匹配字符之后的偏移量
`public int end(int group)`|返回在以前的匹配操作期间，由给定组多捕获子序列的最后字符之后的偏移量

## 3.2. 研究方法
方法|作用
--|--
`public boolean lookingAt()`|尝试从区域开头开始的输入序列与该模式匹配
`public boolean find()`|尝试查找与该模式匹配的输入序列的下一个子序列
`public boolean find()`|重置此匹配器，然后尝试查找该模式，从指定索引开始的输入序列的下一个子序列
`public boolean matches()`|尝试将整个区域与模式相匹配

## 3.3. 替换方法
方法|作用
--|--
`public Matcher appendReplacement(StringBuffer sb,String replacement)`|实现非终端添加和替换步骤
`public StringBuffer appendTail(StringBuffer sb)`|实现终端的添加和替换步骤
`public String replaceAll(String replacement)`|替换模式与给定替换字符串相匹配的输入序列的每个子序列
`public boolean replaceFirst(String replacement)`|替换模式与给定替换字符串匹配的输入序列的第一个子序列
`public static String quoteReplacement(String s)`|返回指定字符串的字面替换字符串。这个方法返回一个字符串

## 3.4. 返回指定位置
1. `start()`:返回当前相应的字符串的首字符位置
2. `end()`:返回当前对应字符串的末字符位置
<a href ="https://www.runoob.com/java/java-regular-expressions.html">接着</a>

# 4. 正则表达式注意
1. 抓取的元组不能放在最后，必须放上确定字符确定如何抓取
2. ()内的部分进行抓取，其余部分不抓取。

# 5. 扩展阅读
<a href = "https://www.cnblogs.com/hd92/p/11322415.html">JAVA正则表达式</a>