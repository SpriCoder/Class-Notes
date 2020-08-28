java变量类型——BigDecimal
---
1. java在java.math包中提供的API类BigDecimal，用来对超过16位有效位的数进行精确的运算。
2. float和double只能用来做科学计算和工程计算，而在商业运算中我们要使用BigDecimal

<!-- TOC -->

- [1. 获得BigDecimal类的对象](#1-获得bigdecimal类的对象)
- [2. BigDecimal类的运算](#2-bigdecimal类的运算)
  - [2.1. 加法](#21-加法)
  - [2.2. 减法](#22-减法)
  - [2.3. 乘法](#23-乘法)
  - [2.4. 除法](#24-除法)
  - [2.5. 绝对值](#25-绝对值)
  - [2.6. 带余除法](#26-带余除法)
- [其他的BigDecimal的API](#其他的bigdecimal的api)
- [3. BigDecimal舍入模式](#3-bigdecimal舍入模式)
  - [3.1. ROUND_UP](#31-round_up)
  - [3.2. ROUND_DOWN](#32-round_down)
  - [3.3. ROUND_CEILING](#33-round_ceiling)
  - [3.4. ROUND_FLOOR](#34-round_floor)
  - [3.5. ROUNG_HALF_UP](#35-roung_half_up)
  - [3.6. ROUND_HALF_DOWN](#36-round_half_down)
  - [3.7. ROUND_HALF_EVEN](#37-round_half_even)
  - [3.8. ROUND_UNNESSARY](#38-round_unnessary)
  - [参考文献](#参考文献)
- [4. 转换为相应类型进行返回](#4-转换为相应类型进行返回)

<!-- /TOC -->

# 1. 获得BigDecimal类的对象
1. 将BigDecimal转换存Double:
    + 推荐使用`new BigDecimal(num+"")`
    + 尽量避免使用`new BigDecimal(num)`，因为这样会有比较多的无效位数。
2. 除此以外，我们可以通过写数字的值或者string来创建BigDecimal类型。
    + 尽量选择string因为其精度高

# 2. BigDecimal类的运算
1. 必须都是BigDecimal对象才能被使用。

## 2.1. 加法
1. add()函数

## 2.2. 减法
1. substract()函数

## 2.3. 乘法
1. multiply()函数

## 2.4. 除法
1. divide(BigDecimal,scale,舍入方式)函数
    + 在指定情况下，我们需要指定scale位数来确定精度。

## 2.5. 绝对值
1. abs()函数

## 2.6. 带余除法
1. `public BigDecimal[] divideAndRemainder(BigDecimal divisor)`
2. 接受一个BigDecimal对象作为参数，该参数即为除数，返回一个BigDecimal数组。
    + 数组中包含两个元素:第一个元素为两数相除的商，第二个元素是除数。

# 其他的BigDecimal的API
1. `stripTrailingZeros()`将其格式化一个相等的，但去掉了末尾0的BigDecimal

# 3. BigDecimal舍入模式
1. 属于java.math.RoundingMode

## 3.1. ROUND_UP
1. 舍入远离零的舍入模式。在丢弃非零部分之前始终增加数字。(始终对非零舍弃部分前面的数字加1)
2. 这个舍入模式始终不会减少计算值的大小

## 3.2. ROUND_DOWN
1. 接近零的舍入模式。在对齐某部分之前始终不增加数字(从不对舍弃部分前面的数字加1，即截断)。
2. 这个舍入模式始终不会增加计算值的大小。

## 3.3. ROUND_CEILING
1. 接近正无穷大的舍入模式。如果为正，和ROUND_UP相同。若为负，则相反。
2. 此舍入模式始终不会减少计算值。

## 3.4. ROUND_FLOOR
1. 接近负无穷大的舍入模式。如果为正，则舍入行为和ROUND_DOWN相同。若为负，则和ROUND_UP相同。
2. 此舍入模式始终不会增加计算值。

## 3.5. ROUNG_HALF_UP
1. 向“最接近的”数字舍入，如果与两个相邻数字的距离相等，则为向上舍入的舍入模式。如果舍弃部分 >= 0.5，则舍入行为与 ROUND_UP 相同;否则舍入行为与 ROUND_DOWN 相同。
2. 也就是我们学的四舍五入。

## 3.6. ROUND_HALF_DOWN
1. 向“最接近的”数字舍入，如果与两个相邻数字的距离相等，则为上舍入的舍入模式。
如果舍弃部分 > 0.5，则舍入行为与 ROUND_UP 相同;否则舍入行为与 ROUND_DOWN 相同(五舍六入)。

## 3.7. ROUND_HALF_EVEN
1. 向“最接近的”数字舍入，如果与两个相邻数字的距离相等，则向相邻的偶数舍入。如果舍弃部分左边的数字为奇数，则舍入行为与 ROUND_HALF_UP 相同;
2. 如果为偶数，则舍入行为与 ROUND_HALF_DOWN 相同。
3. 注意，在重复进行一系列计算时，此舍入模式可以将累加错误减到最小。
4. 此舍入模式也称为“银行家舍入法”，主要在美国使用。四舍六入，五分两种情况。如果前一位为奇数，则入位，否则舍去。以下例子为保留小数点1位，那么这种舍入方式下的结果。
    + 1.15>1.2 1.25>1.2
    
## 3.8. ROUND_UNNESSARY
1. 断言请求的操作具有精确的结果，因此不需要舍入。如果对获得精确结果的操作指定此舍入模式，则抛出ArithmeticException。

## 参考文献
1. <a href = "https://www.cnblogs.com/jpfss/p/9987319.html">星朝</a>
2. <a href = "https://blog.csdn.net/bailu666666/article/details/79829902">java大数类</a>

# 4. 转换为相应类型进行返回
1. toString()将BigDecimal对象的数值转换成字符串。    
2. doubleValue()将BigDecimal对象中的值以双精度数返回。   
3. floatValue()将BigDecimal对象中的值以单精度数返回。   
4. longValue()将BigDecimal对象中的值以长整数返回。    
5. intValue()将BigDecimal对象中的值以整数返回。