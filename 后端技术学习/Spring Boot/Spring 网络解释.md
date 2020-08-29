spring 网络解释
---

<!-- TOC -->

- [1. spring解析URL参数的多种方式](#1-spring解析url参数的多种方式)
  - [1.1. 方式一:表单的参数写到方法形参](#11-方式一表单的参数写到方法形参)
  - [1.2. 方式二:HttpServletRequest](#12-方式二httpservletrequest)
  - [1.3. 方式三:通过Bean来接收](#13-方式三通过bean来接收)
    - [1.3.1. 方式四:通过@PathVariable获取路径中的参数](#131-方式四通过pathvariable获取路径中的参数)
    - [1.3.2. 方式五:适用@ModeAttribute注解获得POST请求的FORM表单数据](#132-方式五适用modeattribute注解获得post请求的form表单数据)
    - [1.3.3. 方式六:用注解@RequestParam绑定参数到方法入参](#133-方式六用注解requestparam绑定参数到方法入参)
- [2. 参考](#2-参考)

<!-- /TOC -->

# 1. spring解析URL参数的多种方式

## 1.1. 方式一:表单的参数写到方法形参
1. 可以直接把表单的参数写到Controller的对应方法的形参中
2. 目前这种方式适用于get方式提交，不适用于post方式提交
```java
@RequestMapping("/addUser1")
public String addUser1(String username,String password) {
```

## 1.2. 方式二:HttpServletRequest
1. 支持用post、get方式来进行。
```java
@RequestMapping("/addUser2")
public String addUser2(HttpServletRequest request) {
```

## 1.3. 方式三:通过Bean来接收
1. 建立一个和表单参数对应的bean，用这个bean来接受(省去get和set方法)
```java
public class UserModel {
    private String username;
    private String password;
}
@RequestMapping("/addUser3")
public String addUser3(UserModel user) {
}
```
### 1.3.1. 方式四:通过@PathVariable获取路径中的参数
```java
@RequestMapping(value="/addUser4/{username}/{password}",method=RequestMethod.GET)
public String addUser4(@PathVariable String username,@PathVariable String password) {
```

### 1.3.2. 方式五:适用@ModeAttribute注解获得POST请求的FORM表单数据
1. JSP表单部分:
```js
<form action ="<%=request.getContextPath()%>/demo/addUser5" method="post"> 
    用户名:&nbsp;<input type="text" name="username"/><br/>
    密&nbsp;&nbsp;码:&nbsp;<input type="password" name="password"/><br/>
    <input type="submit" value="提交"/> 
    <input type="reset" value="重置"/> 
</form> 
```
2. java的Controller实现
```java
@RequestMapping(value="/addUser5",method=RequestMethod.POST)
public String addUser5(@ModelAttribute("user") UserModel user) {
```

### 1.3.3. 方式六:用注解@RequestParam绑定参数到方法入参
1. username不存在的话，可以为这个元注解，添加required的参数，设置为false
```java
@RequestMapping(value="/addUser6",method=RequestMethod.GET)
public String addUser6(@RequestParam("username") String username,@RequestParam("password") String password) {
```

# 2. 参考
1. <a href = "https://www.cnblogs.com/Mr-Rocker/p/10430532.html">Spring中解析URL参数的两种方式</a>