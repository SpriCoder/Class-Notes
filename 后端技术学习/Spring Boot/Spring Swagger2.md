Spring Swagger2
---
1. 这部分是用来完成动态的生成前后端的API接口文档，保证开发高效

<!-- TOC -->

- [1. pom.xml的依赖](#1-pomxml的依赖)
  - [1.1. 添加配置类](#11-添加配置类)
  - [1.2. 使用swagger2注释方法](#12-使用swagger2注释方法)
    - [1.2.1. @Api](#121-api)
    - [1.2.2. @ApiOperation()](#122-apioperation)
    - [1.2.3. @ApiParam()](#123-apiparam)
    - [1.2.4. @ApiModel()](#124-apimodel)
    - [1.2.5. @ApiModelProperty()](#125-apimodelproperty)
    - [1.2.6. @ApiImplicitParam()](#126-apiimplicitparam)
    - [1.2.7. @ApiImplicitParams()](#127-apiimplicitparams)
  - [1.3. 查看API](#13-查看api)

<!-- /TOC -->

# 1. pom.xml的依赖
```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

## 1.1. 添加配置类
1. 我们建立swaggerConfig类

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket customDocket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("包名"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        Contact contact = new Contact("团队名", "www.my.com", "my@my.com");
        return new ApiInfoBuilder()
                .title("文档标题")
                .description("文档描述")
                .contact(contact)   // 联系方式
                .version("1.1.0")  // 版本
                .build();
    }
}
```

## 1.2. 使用swagger2注释方法
```java
package ns.mental.advi.controller;


import io.swagger.annotations.*;
import ns.mental.advi.entity.UserEntity;
import ns.mental.advi.service.impl.UserServiceImpl;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;


@RestController
@RequestMapping("/testBoot")
public class UserController {

    @Autowired
    private UserServiceImpl userService;


    @ApiOperation(value = "查询用户", response = UserEntity.class, httpMethod = "GET" ,notes = "返回一个User对象")
    @RequestMapping("getUser/{id}")
    public String GetUser(@PathVariable int id){
        return userService.SelectByID(id).toString();
    }
}
```

### 1.2.1. @Api
1. 用于注解类，标识这个类为Swagger的资源 
2. 常用的参数有Value 和 Tags，都是说明的作用，但是value可以用Tags替代，当有多个Tags的时候会生成多个list

### 1.2.2. @ApiOperation()
1. 用于注解方法 
2. 常用的参数有value(描述方法),notes(提示内容),response(返回对象),httpMethod(具体是什么请求)

### 1.2.3. @ApiParam()
1. 用于方法参数等的说明，表示对参数的添加元数据，不用的话会自动检索代码中的信息 
2. name(参数名),value(参数说明),required(是否必填)

### 1.2.4. @ApiModel()
1. 用于类;表示对类进行说明，用于参数用实体类接收
2. value(表示对象名),description(描述),都可省略

### 1.2.5. @ApiModelProperty()
1. 用于方法，字段； 表示对model属性的说明或者数据操作更改 
2. value(字段说明),name(重写属性名字),dataType(重写属性类型),required(是否必填),example(举例说明),hidden(隐藏)

### 1.2.6. @ApiImplicitParam()
用于方法表示单独的请求参数

### 1.2.7. @ApiImplicitParams()
1. 用于方法，包含多个 @ApiImplicitParam 
2. name(参数名字),value(参数说明),dataType(数据类型) ,paramType(参数类型) ,example(举例说明)

## 1.3. 查看API
`http://localhost:8080/swagger-ui.html`