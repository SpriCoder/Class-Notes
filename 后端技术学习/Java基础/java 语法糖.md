Java 语法糖
---

# 1. Java判断字符串你是否全为汉字
```java
String str1 = "java判断是否quan为汉字"  
String str2 = "全为汉字"  
String reg = "[\\u4e00-\\u9fa5]+"  
boolean result1 = str1.matches(reg)//false  
boolean result2 = str2.matches(reg)//true
```

1. 参考:<a href = "https://blog.csdn.net/h082602/article/details/73251446">Java判断字符串全是汉字</a>