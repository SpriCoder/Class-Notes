Spring ResponseEntity
---
1. ResponseEntity处理http响应

<!-- TOC -->

- [1. ResponseEntity](#1-responseentity)
  - [1.1. 内嵌接口](#11-内嵌接口)
  - [1.2. 常见相应响应](#12-常见相应响应)
    - [1.2.1. status()的使用](#121-status的使用)
  - [1.3. 简单使用](#13-简单使用)
  - [1.4. 设置Http响应头](#14-设置http响应头)

<!-- /TOC -->

# 1. ResponseEntity
1. 标识了整个http响应:状态码、头部信息以及相应体内容
2. 如果要使用ReponseEntity,必须在请求点返回，通常在spring rest中实现。

## 1.1. 内嵌接口
1. HeadersBuilder:不能设置任何响应体属性
2. BodyBuilder(是上面的子接口)

```java
@GetMapping("/hello")
ResponseEntity<String> hello() {
    return ResponseEntity.ok("Hello World!");
}
```

## 1.2. 常见相应响应
1. BodyBuilder accepted();
2. BodyBuilder badRequest();
3. BodyBuilder created(java.net.URI location);
4. HeadersBuilder<?> noContent();
5. HeadersBuilder<?> notFound();
6. BodyBuilder ok();
7. BodyBuilder status(HttpStatus status)
8. BodyBuilder status(int status) 

### 1.2.1. status()的使用
```java
@GetMapping("/age")
ResponseEntity<String> age(@RequestParam("yearOfBirth") int yearOfBirth) {
    if (isInFuture(yearOfBirth)) {
        return ResponseEntity.badRequest()
            .body("Year of birth cannot be in the future");
    }

    return ResponseEntity.status(HttpStatus.OK)
        .body("Your age is " + calculateAge(yearOfBirth));
}
```

## 1.3. 简单使用
```java
@GetMapping("/age")
ResponseEntity<String> age(@RequestParam("yearOfBirth") int yearOfBirth) {
    if (isInFuture(yearOfBirth)) {
        return new ResponseEntity<>("Year of birth cannot be in the future", HttpStatus.BAD_REQUEST);
    }
    return new ResponseEntity<>("Your age is " + calculateAge(yearOfBirth), HttpStatus.OK);
}
```

## 1.4. 设置Http响应头
```java
//方式一
@GetMapping("/customHeader")
ResponseEntity<String> customHeader() {
    HttpHeaders headers = new HttpHeaders();
    headers.add("Custom-Header", "foo");
    return new ResponseEntity<>("Custom header set", headers, HttpStatus.OK);
}
//方式二
@GetMapping("/customHeader")
ResponseEntity<String> customHeader() {
    return ResponseEntity.ok()
        .header("Custom-Header", "foo")
        .body("Custom header set");
}
```

1. `.body()`返回的是ResponseEntity，而并不是BodyBuilder，需要在最后进行调用。