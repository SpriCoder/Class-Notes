statistics
---
1. 该模块为数学（限定为实数）数据提供了计算数学统计量的函数。


函数名|函数功能|备注
--|--|--
mean(list)|平均值|-
harmonix_mean()|数据的调和平均值|（如果存在X1、X2……Xn，xi≠0、i=1,2,..,n，H= n/(1/X1+1/X2+……+1/Xn)。其中H即为X1、X2……Xn的调和平均值。）
mode(list)|众数|-
median()|数据的中位数|-
median_low()|数据的第一个中位数（总数为偶数时有两个中位数）|-
median_high()|数据的第二个中位数|-
median_grouped()|分组数据的中位数的均值|-
mode()|离散数据的模式（即数据中最常见的值）。|-
pstdev()|数据总体的标准差|-
pvariance()|数据总体的方差|-
stdev()|数据样本的标准差|-
variance()|数据样本的方差|-