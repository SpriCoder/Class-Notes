持久层框架-Mybatis
---

<!-- TOC -->

- [1. Mybatis和数据库的交互方式](#1-mybatis和数据库的交互方式)
  - [1.1. 使用原始的Dao来调用mybatis](#11-使用原始的dao来调用mybatis)
  - [1.2. Mapper动态代理](#12-mapper动态代理)
- [2. MyBatis映射文件](#2-mybatis映射文件)
  - [2.1. select语句](#21-select语句)
  - [2.2. insert && update && delete](#22-insert--update--delete)
  - [2.3. sql](#23-sql)
- [3. 动态SQL](#3-动态sql)
  - [3.1. if](#31-if)
  - [3.2. choose,when,otherwise](#32-choosewhenotherwise)
  - [3.3. trim,where,set](#33-trimwhereset)
  - [3.4. foreach](#34-foreach)
  - [3.5. bind](#35-bind)
- [4. 多数据库支持](#4-多数据库支持)
- [5. 动态SQL中的可插拔脚本语言](#5-动态sql中的可插拔脚本语言)
- [6. 导出导入数据到数据库](#6-导出导入数据到数据库)
  - [6.1. 导出数据库](#61-导出数据库)
  - [6.2. 导入服务器](#62-导入服务器)
- [7. mybatis-generator](#7-mybatis-generator)
  - [7.1. 具体实现](#71-具体实现)
    - [7.1.1. 在pom文件中添加配件配置](#711-在pom文件中添加配件配置)
    - [7.1.2. 在resource下创建generatorConfig.xml文件](#712-在resource下创建generatorconfigxml文件)
    - [7.1.3. 出现的问题](#713-出现的问题)
- [8. 使用标签结合Mybatis](#8-使用标签结合mybatis)
- [9. 参考](#9-参考)

<!-- /TOC -->
**持久层框架mybatis**

# 1. Mybatis和数据库的交互方式

## 1.1. 使用原始的Dao来调用mybatis
1. 需要定义dao接口
2. 需要实现dao接口
3. 在dao接口实现类上注入sqlSessionFactory,创建时需要读取mapper.xml到内存
4. 创建sqlSession对象调用mapper中的sql语句(sqlSession.insert(statementID,sql)),操作结束后需要手工提交，释放资源，返回结果
5. 存在的问题：
    1. dao实现类方法中存在大量模板方法
    2. 通过sqlSession进行编码的数据库操作方法存在id的硬编码
    3. sqlSession方法使用泛型，运行时错误
## 1.2. Mapper动态代理
1. 需要实现mapper接口
2. 定义mapper.xml是需要保证
    1. namespace和mapper接口方法的路径相同
    2. statementID和mapper接口中定义的方法相同
    3. paramerType和mapper接口中定义的参数类型相同
    4. 返回的resultType要和mapper接口中定义的返回结果相同

# 2. MyBatis映射文件
1. MyBatis最强大之处就在于它的映射语句
2. 映射文件的几个顶级元素：
    1. cache:对给定命名空间的缓存配置
    2. cache-ref:对其他命名空间缓存配置的引用
    3. resultMap:描述如何从数据库的结果中来加载对象
    4. sql:可被其他语句引用的可重用语句块
    5. insert:映射插入语句
    6. update:映射更新语句
    7. delete:映射删除语句
    8. select:映射查询语句

## 2.1. select语句
```xml
<select id = "select" parameterType = "int" resultType = "hashmap">
    SELECT * FROM PERSON WHERE ID = #{id}
</select>
```
1. 注意参数的符号
2. 允许更多的参数：<a href ="https://www.w3cschool.cn/mybatis/f4uw1ilx.html">详见</a>

## 2.2. insert && update && delete
1. 这些都和select比较相近
2. 如果使用的数据库(MySQL)可以自动生成主键的字段，那么可以设置:`useGeneratedKeys = "true"`

## 2.3. sql
1. 用于定义可重用的SQL代码代码段
2. 可以被使用在别的之中
3. 未完待续
```xml
<include refid = "name"></include>
```

# 3. 动态SQL

## 3.1. if
1. 条件判断的一部分，往往放置在where中
2. 例子：
```xml
<select id ="name" resultType = "int">
    SELECT * FROM BLOG WHERE state = "new"
    <if test = "title != null">
        AND title like #{title}
    </if>
</select>
```

## 3.2. choose,when,otherwise
1. 类似java中的switch
2. 例子：
```xml
<select id = "find" resultType = "PRESON">
    SELECT * FROM PERSON WHERE name = "job"
    <choose>
        <when test = "title != null">
        </when>
        <when test = "author != null">
        </when>
        <otherwise>
        </otherwise>
    </choose>
</select>
```

## 3.3. trim,where,set
1. where:防止没有条件符合造成的炸裂
2. 例子：
```xml
<select id ="name" resultType = "int">
    SELECT * FROM BLOG 
    <where>
        <if>
        </if>
    </where>
</select>
```
3. trim：自定义where元素的功能
    1. prefixOverrides属性中的内容会被替换成prefix
    2. suffixOverrides属性中的会被添加进去，比如逗号
```xml
<trim prefix = "WHERE" prefixOverrides = "AND|OR">
```
4. set:动态更新语句
    1. 会动态设置SET关键字，同时删除无关的逗号
```xml
<update id ="name">
    update Author
    <set>
        <if test ="">
        </if>
    </set>
</update>
```
## 3.4. foreach
1. foreach：对一个集合进行遍历
    1. 声明在集合体中使用的集合项，也允许你放置分隔符和开始结尾
    2. index是键
    3. item是值
```xml
<select id = "name" resultType = "blog.post">
    SELECT *
    FROM POST P
    WHERE ID in
    <foreach item = "item" index = "index" collection = "list" open = "(" separator = "," close = ")">
        #{item}
    </foreach>
</select>
```
## 3.5. bind
1. 可以冲OGNL表达式中创建一个变量绑定到上下文
2. 例子：
```xml
<select id = "name" resultType= "blog.post">
    <bind name = "pattern" value = "'%" + _parameter.getTitle()+"%"/>
</select>
```

# 4. 多数据库支持

# 5. 动态SQL中的可插拔脚本语言

# 6. 导出导入数据到数据库

## 6.1. 导出数据库
1. 进入Mysql安装地点的bin文件夹下：`mysqldump -u [用户名] -p  [要导出的数据库]>[导出的路径//[文件名].txt]`
    + 例如:`mysqldump -u root -p jyb > jyb.txt   (输入后会让你输入进入MySQL的密码)`

## 6.2. 导入服务器
1. 进入Xftp,将刚刚的.txt文件上传到服务器
2. 创建数据库:`create database name;`
3. 将刚刚的文件导入:`source [所在的路径//*.txt]`
    + 例如:`source /root/jyb.txt;`
4. 确认数据:`show databases;`

# 7. mybatis-generator
1. mybatis-generator是一款mybatis的自动代码生成工具，可以通过配置，快速生成mapper和xml文件。

## 7.1. 具体实现

### 7.1.1. 在pom文件中添加配件配置
```xml
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.2</version>
    <configuration>
        <verbose>true</verbose>
        <overwrite>true</overwrite>
    </configuration>
    <executions>
        <execution>
            <id>Generate MyBatis Artifacts</id>
            <goals>
                <goal>generate</goal>
            </goals>
        </execution>
    </executions>
    <dependencies>
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.2</version>
        </dependency>
    </dependencies>
</plugin>
```

### 7.1.2. 在resource下创建generatorConfig.xml文件
1. 文件内容为
```xml

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE generatorConfiguration

        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"

        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <!-- 数据库驱动:选择你的本地硬盘上面的数据库驱动包-->

    <classPathEntry  location="C:\Users\Administrator\Downloads\mysql-connector-java-8.0.13.jar"/>

    <context id="DB2Tables"  targetRuntime="MyBatis3">

        <commentGenerator>

            <property name="suppressDate" value="true"/>

            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->

            <property name="suppressAllComments" value="true"/>

        </commentGenerator>

        <!--数据库链接URL，用户名、密码

           1.一般jdbc数据库的版本6.x以上，都是com.mysql.cj.jdbc.Driver  其他的低版本就是com.mysql.cj.jdbc.Driver

         -->

        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver" connectionURL="jdbc:mysql://127.0.0.1/cigit2019?serverTimezone=GMT" userId="root" password="1234">

        </jdbcConnection>

        <javaTypeResolver>

            <property name="forceBigDecimals" value="false"/>

        </javaTypeResolver>

        <!-- 生成模型的包名和位置-->

        <javaModelGenerator targetPackage="com.example.cigit.dao" targetProject="src/main/java">

            <property name="enableSubPackages" value="true"/>

            <property name="trimStrings" value="true"/>

        </javaModelGenerator>

        <!-- 生成映射文件的包名和位置-->

        <sqlMapGenerator targetPackage="mapping" targetProject="src/main/resources">

            <property name="enableSubPackages" value="true"/>

        </sqlMapGenerator>

        <!-- 生成DAO的包名和位置-->

        <javaClientGenerator type="XMLMAPPER" targetPackage="com.example.cigit.mapping" targetProject="src/main/java">

            <property name="enableSubPackages" value="true"/>

        </javaClientGenerator>

        <!-- 要生成的表 tableName是数据库中的表名或视图名 domainObjectName是实体类名-->

        <table tableName="t_user" domainObjectName="User" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>

    </context>

</generatorConfiguration>
```
1. 在如上模板中需要修改的位置
    1. classPathEntry---数据库驱动:选择你的本地硬盘上面的数据库驱动包，具体可以在https://mvnrepository.com/artifact/mysql/mysql-connector-java下载
	2. jdbcConnection---数据库连接的信息，用户名密码等
	3. javaModelGenerator---生成模型的包名和位置
	4. sqlMapGenerator---生成生成映射文件的包名和位置
	5. javaClientGenerator---生成DAO的包名和位置
	6. table--- 要生成的表 tableName是数据库中的表名或视图名 domainObjectName是实体类名

### 7.1.3. 出现的问题
1. 具体参考参考中的相应链接

# 8. 使用标签结合Mybatis
注解|功能|备注
--|--|--
@Update|对应更新数据库|-
@Select|对应选择数据库中项|-
@Insert|向数据库添加项|-
@Delete|删除数据库中项|-

# 9. 参考
1. <a href = "https://blog.csdn.net/j18423532754/article/details/86488448">用mybatis的generator自动生成代码</a>
2. <a href = "https://www.cnblogs.com/expiator/p/8652094.html">Mybatis自动生成xml文件、dao接口、实体类</a>
3. <a href = "https://blog.csdn.net/qq_36881106/article/details/82143232">idea中mybatis自动生成工具及用法</a>