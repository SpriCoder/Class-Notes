Spring boot 出现的部分问题 以及对应的解决办法
---

<!-- TOC -->

- [1. JSON解析错误](#1-json解析错误)
  - [1.1. 例子](#11-例子)
  - [1.2. 参考文献](#12-参考文献)
  - [1.3. 出现400相应的问题](#13-出现400相应的问题)
- [2. @Autowired 的 null 问题](#2-autowired-的-null-问题)
  - [2.1. websocket 和 spring boot 框架结合的问题](#21-websocket-和-spring-boot-框架结合的问题)
- [3. springboot中的ErrorPageFilter](#3-springboot中的errorpagefilter)
  - [3.1. 参考](#31-参考)
- [4. @RequestParam(required = false) 带来的空指针问题](#4-requestparamrequired--false-带来的空指针问题)
- [5. Spring框架没有反应的问题](#5-spring框架没有反应的问题)
- [6. 上传文件时:The field file exceeds its maximum permitted size of 1048576 bytes.](#6-上传文件时the-field-file-exceeds-its-maximum-permitted-size-of-1048576-bytes)
- [7. 时区问题](#7-时区问题)
- [8. 数据库问题](#8-数据库问题)
  - [8.1. JPA](#81-jpa)

<!-- /TOC -->

# 1. JSON解析错误
>org.springframework.http.converter.HttpMessageNotWritableException: Could not write content: No serializer found for class net.sf.ezmorph.MorpherRegistry and no properties discovered to create BeanSerializer
1. 这种错误十分常见，往往是指将JSON字符串转成javaBean时，可以直接进行转换。或者我们并没有能够提供相应的get和set方法

## 1.1. 例子
```java
public class Student implements java.io.Serializable{
    private static final long serialVersionUID = 1L;
    private String sname;
    private Integer age;
    public String getSname() {
        return sname;
    }
    public void setSname(String sname) {
        this.sname = sname;
    }
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }    
}
```

1. 由字符串转javabean时可以使用以下方法:
```java
String str = "[{\"sname\":\"admin\",\"age\":20}]";
////接收{}对象，此处接收数组对象会有异常,故需要解决
if (str.indexOf("[") != -1) {
    str = str.replace("[", "");
}
if (str.indexOf("]") != -1) {
    str = str.replace("]", "");
}
JSONObject jobj = JSONObject.fromObject(str);
Student s = (Student) JSONObject.toBean(jobj，Student.class);
```
2. 上述方法存在问题:如果存在复杂类型:比如List，Map,ArrayList就不可以了，比如如下
```java
public class Student implements java.io.Serializable{
    private static final long serialVersionUID = 1L;
    private String sname;
    private Integer age;
    private List < String > courses;
    public String getSname() {
        return sname;
    }
    public void setSname(String sname) {
        this.sname = sname;
    }
    public Integer getAge() {
        return age;
    }
    public void setAge(Integer age) {
        this.age = age;
    }
    public List getCourses()
    {
        return courses;
    }
    public void setCourses(List courses)
    {
        this.courses = courses;
    }    
}
```

3. 此时我们就需要这么来写了(如何仍然包含其他复杂类型，应该继续向classMap中放置对应的key和类型)
```java
Map classMap = new HashMap();
classMap.put("courses",String.class);
Student student =( Student) JSONObject.toBean(str, Student.class, classMap);
```


## 1.2. 参考文献
1. <a href = "https://www.liangzl.com/get-article-detail-130462.html">JSON解析错误</a>

## 1.3. 出现400相应的问题
>问题描述:在我们展开的过程中，经过断点调试，我们确认，如果实体类提供了含参构造方法，则不会默认提供无参构造方法，从而会导致我们的参数传递和JSON化失败，因此我们需要检查这部分的情况。
1. 所以请务必注意，在提供含参构造方法的同时，我们一定要同时提供无参构造方法。

# 2. @Autowired 的 null 问题

## 2.1. websocket 和 spring boot 框架结合的问题
>@Autowired在被@ServerEndPoint和@Component修饰的类中，无法自动装配，显示null
1. 因为被spring boot的IOC容器管理，Bean是被单件管理的，而websocket则会被多装配。
2. 所以我们需要使用@Autowired装配给set方法,并且变为静态变量。
```java
@ServerEndPoint("/video")
@Component
public class videoPlayer(){
    private static StudentDao studentDao;
    @Autowired
    public void setStudentDao(StudentDao studentDao){
        videoPlayer.studentDao = studentDao;
    }
}
```

# 3. springboot中的ErrorPageFilter
1. ErrorPageFilter是SpringBoot在1.4.0版本提供的一个类，本质上是一个Filter。 它的作用主要有两方面：
    1. 提供应用程序注册ErrorPage的接口，此时它的角色是：ErrorPageRegistry
    2. 处理应用程序异常，根据异常的类型转发到对应的ErrorPage页, 从而不依赖部署的容器错误处理机制
2. <a href = "https://leokongwq.github.io/2017/04/07/spring-ErrorPageFilter.html">SpringBoot异常处理之ErrorPageFilter</a>

## 3.1. 参考
1. <a href = "https://leokongwq.github.io/2017/04/07/spring-ErrorPageFilter.html">Springboot异常处理值ErrorPageFilter</a>

# 4. @RequestParam(required = false) 带来的空指针问题
1. 这种问题，我们要用Integer等封装主类型，然后之后根据null与否进行不同业务逻辑

# 5. Spring框架没有反应的问题
1. 检查防火墙:是不是阿里防火墙阻断了请求。

# 6. 上传文件时:The field file exceeds its maximum permitted size of 1048576 bytes.
1. 上传文件的大小大于了Spring本身默认的1M的文件大小
2. 可以修改(在配置文件中application.properities中)
```
spring.servlet.multipart.max-file-size = 10MB  
spring.servlet.multipart.max-request-size=100MB
```

# 7. 时区问题
>在东八区进行操作，显示的时间和当地时间正好差8个小时，往往是Mybatis的链接中的时区的问题
1. 解决:`spring.datasource.url=jdbc:mysql://localhost:3306/lmg-test?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC&zeroDateTimeBehavior=convertToNull`改为`spring.datasource.url=jdbc:mysql://localhost:3307/lmg-test?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=GMT%2B8&zeroDateTimeBehavior=convertToNull`
2. 也就是将:`serverTimezone`的`UTC`改为`GMT%2B8`
3. <a href = "https://blog.csdn.net/weixin_42322648/article/details/105116600">参考</a>

# 8. 数据库问题

## 8.1. JPA
> Can not issue data manipulation statements with executeQuery().

1. 这个方法是update方法，且只被Query修饰
2. 解决方案:使用`@Modifying`和`@Transactional`来进行修饰

>自定义查询
1. 需要使用`@Query`参数设置SQL语句，并且`nativeQuery`设置为true