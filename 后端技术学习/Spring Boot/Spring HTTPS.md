Spring HTTPS
---
1. 在正常的应用场景之下，HTTPS请求会使得我们的应用程序更加安全。

<!-- TOC -->

- [1. SSL证书](#1-ssl证书)
  - [1.1. 自制HTTPS证书](#11-自制https证书)
  - [1.2. 阿里云等证书](#12-阿里云等证书)
  - [1.3. 证书的位置](#13-证书的位置)
- [2. POM文件设置](#2-pom文件设置)
- [3. HTTP自动转换HTTPS请求](#3-http自动转换https请求)
- [4. 参考](#4-参考)

<!-- /TOC -->

# 1. SSL证书
1. SSL证书是使用HTTPS请求中必不可少的一部分

## 1.1. 自制HTTPS证书
1. KeyTool(Java自带):`keytool -genkey -alias tomcat  -storetype PKCS12 -keyalg RSA -keysize 2048  -keystore keystore.p12 -validity 3650`
   1. storetype 指定密钥仓库类型
   2. keyalg 生证书的算法名称，RSA是一种非对称加密算法
   3. keysize 证书大小
   4. keystore 生成的证书文件的存储路径
   5. validity 证书的有效期

## 1.2. 阿里云等证书
1. 阿里云、腾讯云等购买了服务器都可以申请免费SSL证书，免费证书大致是够用的
2. <a href = "https://blog.csdn.net/She_lock/article/details/80984992">阿里云免费ssl证书（PFX格式证书）安装</a>

## 1.3. 证书的位置
1. 放置到项目的根目录上。

# 2. POM文件设置
```yml
server:
  port: 8000 # 默认HTTPS端口号
  ssl:
    key-store: keyname.suffix
    enabled: true
    key-store-password: password
    key-store-type: JKS
```

# 3. HTTP自动转换HTTPS请求
```java
@RestController
@SpringBootApplication
public class TestApplication {

    public static void main(String[] args) {
        SpringApplication.run(TestApplication.class, args);
    }
    @Bean
    public EmbeddedServletContainerFactory servletContainer() {
        TomcatEmbeddedServletContainerFactory tomcat = new TomcatEmbeddedServletContainerFactory() {
            @Override
            protected void postProcessContext(Context context) {
                SecurityConstraint constraint = new SecurityConstraint();
                constraint.setUserConstraint("CONFIDENTIAL");
                SecurityCollection collection = new SecurityCollection();
                collection.addPattern("/*");
                constraint.addCollection(collection);
                context.addConstraint(constraint);
            }
        };
        tomcat.addAdditionalTomcatConnectors(httpConnector());
        return tomcat;
    }

    @Bean
    public Connector httpConnector() {
        Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
        connector.setScheme("http");
        //Connector监听的http的端口号
        connector.setPort(8080);
        connector.setSecure(false);
        //监听到http的端口号后转向到的https的端口号
        connector.setRedirectPort(8081);
        return connector;
    }

}
```

# 4. 参考
1. <a href = "https://www.jianshu.com/p/71cd01fa8438">Spring Boot支持HTTPS请求</a>