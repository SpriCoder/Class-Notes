
<!-- TOC -->

- [1. Network](#1-network)
  - [1.1. web部分](#11-web部分)
- [2. WEB发展的两个阶段：静态、动态](#2-web发展的两个阶段静态动态)
  - [2.1. 静态web：](#21-静态web)
  - [2.2. 动态WEB：](#22-动态web)
  - [2.3. WEB服务器](#23-web服务器)
- [3. Java Web的具体应用](#3-java-web的具体应用)
- [4. 客户端和服务器端的简单应用](#4-客户端和服务器端的简单应用)
  - [4.1. 客户端正常工作的三件事情：](#41-客户端正常工作的三件事情)
  - [4.2. 简单的服务器程序](#42-简单的服务器程序)
    - [4.2.1. 工作方式](#421-工作方式)
    - [4.2.2. Servlet实例](#422-servlet实例)

<!-- /TOC -->

# 1. Network

## 1.1. web部分
1. Internet上供外界访问的Web资源分为：
    1. 静态Web资源:指Web页面中供人们浏览的数据始终是不变的
    2. 动态Web资源:指Web页面中供人们浏览的数据是由人们浏览的数据是由程序产生
    3. 开发静态web资源开发技术：html
    4. 动态web资源开发技术：JSP/Servlet、ASP、PHP
2. Web应用程序
    1. WEB应用程序指供浏览器访问的程序，通常也简称为web应用
    2. 1个web应用由多个静态web资源和动态web资源组成
    3. web应用开发好后，若想供外界访问，需要把web应用所在目录交给web服务器管理，这个过程为虚拟目录的映射。
3. 网络部分学习综述：
    1. fe、http(接口，链接前端和后端)、be
    2. 前端框架：Vue、React、Angular
    3. 后端框架：spring(boot)、nodejs、sql
    4. 后端部分：
        1. request:
            + headers、method、content
        2. response:
            + hearders、body等
# 2. WEB发展的两个阶段：静态、动态

## 2.1. 静态web：
1. 客户端通过Web浏览器经过网络链接到服务器上，使用HTTP协议发起一个请求，告诉服务器现在需要哪一个页面，所有请求交给WEB服务器，然后服务器按照客户的需求，从文件系统取出内容。之后通过WEB服务器返回给客户端，客户端接收到内容之后经过浏览器渲染解析，得到显示的效果。
    + 缺点一：Web页面中的内容无法动态更新，所有的用户每时每刻看见的内容和最终的效果都是一样的
    + 缺点二：静态WEB无法连接数据库，无法实现和用户的交互

## 2.2. 动态WEB：
1. 特点：WEB的界面展示效果因时因人而变
2. 动态WEB中，程序依旧使用客户端和服务端，客户端依然使用浏览器，通过网络连接到服务器，通过HTTP协议发起请求，但是所有的请求都先经过WEB Server Plugin来处理，次产检主要是用来区分静态资源还是动态资源
3. 动态WEB应用的实现手段：
    1. Microsoft ASP、ASP.NET
    2. PHP
    3. JAVA Servlet/JSP

## 2.3. WEB服务器
1. 是指驻留在因特网上某种类型计算机的程序，是可以向发出请求的浏览器提供文档的程序。当Web浏览器连到服务器上并请求文件时，服务器将处理该请求并将文件反馈到该浏览器上，附带的信息会告诉浏览器如何查看该文件。
2. 服务器是一种被动程序，只有当Internet上运行在其他计算机中的浏览器发出请求时，服务器才会相应
3. Tomcat服务器

# 3. Java Web的具体应用
1. 所有网络运行的低层细节都已经由java.net函数库处理掉
2. java web的工作方式：
    1. 客户端连接到服务器
    2. 服务器建立连接并把客户端加到来客清单中
    3. 另一个用户连接上来
    4. 用户A送出信息到聊天服务器上
    5. 聊天服务器将信息送给所有的来宾
    
# 4. 客户端和服务器端的简单应用

## 4.1. 客户端正常工作的三件事情：
1. 如何建立客户端与服务器连接：
    + 使用socket连接：`Socket chatsocket = new Socket("196.164.1.103",5000);`
    + 这种连接需要两个信息：JP的地址和TCP的端口值
    + Socket连接的建立代表两台机器之间存有对方的信息，包括网络地址和TCP的端口号。
        1. TCP端口知识一个16位宽、用来识别服务器上特定程序的数字
        2. 0-65535是所有端口的范围，其中0-1023的TCP端口是保留给已知的特定服务使用，这些端口不应该被使用。
        3. 如果是在互联网上执行自己的服务，则需要先和网管讨论有哪些端口已经被占用，有时候网管会把特定的端口号用防火墙或者其他特定的安全监控机制封锁起来
        4. 如果绑定端口时，端口已经被占用，那么会抛出一个BindException
2. 如何传送信息到服务器：
3. 如何接受来自服务器的信息:
    1. 使用BufferedReader从socket上读取数据：
        1. 建立Socket链接:`Socket chatsocket = new Socket("127.0.0.1",5000);`
        2. 建立连接到Socket上底层输入串流的InputStreamReader:`InputStreamReader stream = new InputStreamReader(charSocket.getInputStream());`
        3. 建立BufferedReader进行读取:
            + `BufferReader reader = new BufferReader(stream);`
            + `Stirng message = reader.readLine();`
    2. 使用PrintWriter写数据到Socket上：
        1. 对服务器建立Socket连接:`Socket chatsocket = new Socket("127.0.0.1",5000);`
        2. 建立链接到Socket的PrintWriter:`PrintWriter writer = new PrintWriter(chatSocket.getOutputStream());`
        3. 写入数据：
            + `writer.println("message to send");`
            + `writer.print("another message");`
4. 成功被调用后，一个流被建立了，在这部分调用中有可能会出状况所以请放入try{}catch{}块中进行操作

## 4.2. 简单的服务器程序
1. 需要的东西：一对socket
    1. 一个会等待用户请求的ServerSocket
    2. 和一个与用户通信用的Socket

### 4.2.1. 工作方式
1. 服务器程序对特定端口创建出ServeSocket
    + `ServeSocket serveSock = new ServeSocket(4242);`
    + 服务器应用程序开始监听
2. 客户端对服务器应用程序建立Socket连接
    + `Socket sock = new Socket("190.165.1.103",4242)`
    + 客户端需要知道IP地址和端口号
3. 服务器创建出和客户端通信的新Socket
    + `Socket sock = serveSock.accept()`
    + accept()方法会在等待用户的Socket连接时闲置着。
    + 使用while(true)来反复循环，直到连接

### 4.2.2. Servlet实例
```java
public class HelloWorldServlet extends HttpServlet {
    private String message;
    
    public void init() throws ServletException {
        message = "Hello World";
    }
    public void doGet(HttpServletRequest request, HttpServletResponse
    response) throws ServletException, IOException {
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        out.println("<h1>" + message + "</h1>");
    }
    public void destroy() {}
}
```