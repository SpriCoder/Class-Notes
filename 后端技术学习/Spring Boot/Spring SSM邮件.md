Spring SSM邮件
---

<!-- TOC -->

- [1. 邮箱配置](#1-邮箱配置)
- [2. 相关的POM依赖](#2-相关的pom依赖)
- [3. 在Spring中进行发送](#3-在spring中进行发送)
  - [3.1. 发送简单文本邮件](#31-发送简单文本邮件)
  - [发送带有附件的邮件](#发送带有附件的邮件)
- [4. 出现的问题](#4-出现的问题)
- [5. 参考](#5-参考)

<!-- /TOC -->

# 1. 邮箱配置
1. 首先要配置你将要使用的发送的邮箱
2. 登录你的邮箱，找到设置->POP3/SMTP/IMAP来进行开启POP3/SMTP服务和IMAP/SMTP服务
3. 之后根据自己的邮箱情况找到开启授权码登录方式，拿到授权码即可。

# 2. 相关的POM依赖
```xml
<!--添加spring支持email 依赖-->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-context-support</artifactId>
  <version>4.3.19.RELEASE</version>
</dependency>

<!--邮件发送-->
<dependency>
  <groupId>com.sun.mail</groupId>
  <artifactId>javax.mail</artifactId>
  <version>1.6.1</version>
</dependency>
```

# 3. 在Spring中进行发送

## 3.1. 发送简单文本邮件
1. 可以将配置文件放置到yml中
```java
import org.springframework.mail.javamail.JavaMailSender;
import org.springframework.mail.javamail.JavaMailSenderImpl;
import org.springframework.mail.javamail.MimeMessageHelper;
import org.springframework.stereotype.Component;

import javax.mail.internet.MimeMessage;
import java.io.BufferedReader;
import java.io.FileReader;

/**
 * @Author stormbroken
 * Create by 2020/07/05
 * @Version 1.0
 **/

@Component
public class MailUtil {
    private static JavaMailSender javaMailSender;
    private static String host = "smtp.qq.com";
    private static String emailFrom;
    private static Integer SMTPPort = 465;//SSL
    private static String coding = "utf-8";
    private static String password;

    private static void initJavaMailSender() throws Exception {
        if(javaMailSender == null) {
            synchronized (MailUtil.class){
                if(javaMailSender == null){
                    JavaMailSenderImpl mailSender = new JavaMailSenderImpl();
                    //外部加载数据
                    FileReader fileReader = new FileReader("filename");
                    BufferedReader bufferedReader = new BufferedReader(fileReader);
                    emailFrom = bufferedReader.readLine();
                    password = bufferedReader.readLine();

                    mailSender.setHost(host);//指定用来发送Email的邮件服务器主机名
                    mailSender.setPort(SMTPPort);//默认端口，标准的SMTP端口
                    mailSender.setUsername(emailFrom);//用户名
                    mailSender.setPassword(password);//就是刚刚设置的授权码

                    //加认证机制
                    Properties javaMailProperties = new Properties();
                    javaMailProperties.put("mail.smtp.auth", true);
                    javaMailProperties.put("mail.smtp.starttls.enable", true);
                    javaMailProperties.put("mail.smtp.starttls.required", true);
                    javaMailProperties.put("mail.smtp.timeout", 5000);
                    javaMailProperties.put("mail.smtp.ssl.enable",true);
                    javaMailProperties.put("mail.smtp.socketFactory.class",javax.net.ssl.SSLSocketFactory.class);

                    mailSender.setDefaultEncoding(coding);
                    mailSender.setJavaMailProperties(javaMailProperties);

                    javaMailSender = mailSender;
                }
            }
        }
    }

    public void sendMessage(String toEmail, String code) throws Exception{
        initJavaMailSender();
        MimeMessage message = javaMailSender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(message,true);
        helper.setFrom(emailFrom);//发件人
        helper.setTo(toEmail);//收件人
        helper.setSubject("主题");//主题

        //不使用HTML
        helper.setText("内容", false);
        //使用HTML
        helper.setText("HTML", true);

        //发送邮件
        javaMailSender.send(message);
    }

}

```

## 发送带有附件的邮件
```java
public void sendMessage(String toEmail, String code) throws Exception{
  initJavaMailSender();
  MimeMessage message = javaMailSender.createMimeMessage();
  MimeMessageHelper helper = new MimeMessageHelper(message,true);
  helper.setFrom(emailFrom);//发件人
  helper.setTo(toEmail);//收件人
  helper.setSubject("主题");//主题

  //不使用HTML
  helper.setText("内容", false);
  //使用HTML
  helper.setText("HTML", true);

  //添加附件
  FileSystemResource resource = new FileSystemResource("F:\\Users\\yd\\Desktop\\test.png");
  helper.addAttachment("test.png", resource);
  
  //发送邮件
  javaMailSender.send(message);
}
```

# 4. 出现的问题
1. 在服务器端不能进行发送:
   1. 检查是不是使用的端口没有开放，可以使用telnet命令进行检查:`telnet smtp.qq.com 25`
   2. 是不是因为必须要SSL的端口465进行发送(25端口不可以)

```yml
mail:
    host: smtp.exmail.qq.com
    username: 邮箱地址
    default-encoding: utf-8
    password: 授权码
    port: 465
    properties:
      mail:
        smtp:
          socketFactory:
            class: javax.net.ssl.SSLSocketFactory
```
2. 端口情况：
   1. 25端口：使用普通简单加密类型的端口
   2. 465端口：使用SSL加密方式的端口
   3. 567端口：使用TSL加密方式的端口


# 5. 参考
1. <a href = "https://blog.csdn.net/qq_22465297/article/details/84035252?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522159391576619195265917851%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=159391576619195265917851&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v3~pc_rank_v2-4-84035252.first_rank_ecpm_v3_pc_rank_v2&utm_term=Spring%E5%A6%82%E4%BD%95%E5%8F%91%E9%80%81%E9%82%AE%E4%BB%B6">SSM 框架整合 spring 发送邮件功能实现！</a>
2. <a href = "https://blog.csdn.net/Victor_Szasz/article/details/104515736?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase">Springboot邮件发送 部署到服务器后邮件不能发送问题</a>
3. </a href = "https://www.cnblogs.com/victorlyw/p/10019625.html">使用 spring封装的javamail linux服务器发送邮件失败解决</a>
4. <a href = "https://blog.csdn.net/z1790424577/article/details/80963219?ops_request_misc=&request_id=&biz_id=102&utm_term=Spring%E5%A6%82%E4%BD%95%E5%8F%91%E9%80%81%E9%82%AE%E4%BB%B6&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-2-80963219">Spring发送邮件</a>