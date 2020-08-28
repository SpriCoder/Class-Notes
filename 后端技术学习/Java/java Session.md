# 1. Session介绍
1. 在WEB开发中，服务器可以为每个用户浏览器创建一个会话对象（Session对象）
2. 一个浏览器独占一个session对象，所以需要保存用户数据时，服务器程序可以把用户数据写到浏览器独占的session中，写出时可以在session中取出用户数据。

# 2. Session和Cookie的主要区别
+ Cookie把用户的数据写给用户的浏览器。
+ Session技术把用户的数据写到数据独用的Session中
+ Session对象由服务器创建，开发人员调用request对象的getSession方法得到session对象。

# 3. Session实现原理

## 3.1. 服务器如何实现一个session为一个用户浏览器服务的？
1. 服务器创建session出来,将session的id号，以cookie的形式写回客户机。

## 3.2. session对象的创建和销毁时机

### 3.2.1. session对象的创建时机
1. 程序中第一次调用request.getSession()方法时会创建一个session，可以用isNew()方法来判断session是不是新创建的

### 3.2.2. session对象的销毁时机
1. session对象默认30分钟没有使用，服务器会自动销毁session，在web.xml文件中可以手动配置session的失效时间