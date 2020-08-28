java_外部扩展库
---

# 1. JSON与JAVA

## 1.1. Java将对象序列化存储成为String

```java
public void JsonToString(){
    //就是即将被序列化的对象
    Object oject = new Object();
    //核心对象
    ObjectMapper mapper = new ObjectMapper();
    //将对象进行序列化
    String json = mapper.writeValueAsString(object);
}
```

## 1.2. 参考
1. <a href = "https://www.cnblogs.com/newcityboy/p/11538020.html">如何将java对象转换成JSON数据</as>