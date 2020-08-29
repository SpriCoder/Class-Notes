Spring SSM邮件
---

<!-- TOC -->

- [1. 相关的POM依赖](#1-相关的pom依赖)
- [2. 配置打算使用的邮箱的SMTP和POP3服务](#2-配置打算使用的邮箱的smtp和pop3服务)
- [3. 在Spring中进行发送](#3-在spring中进行发送)
- [4. 参考](#4-参考)

<!-- /TOC -->

# 1. 相关的POM依赖
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

# 2. 配置打算使用的邮箱的SMTP和POP3服务
1. 见参考一

# 3. 在Spring中进行发送
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
    private static Integer SMTPPort = 25;
    private static String coding = "utf-8";
    private static String password;

    private static void initJavaMailSender() throws Exception {
        if(javaMailSender == null) {
            synchronized (MailUtil.class){
                if(javaMailSender == null){
                    JavaMailSenderImpl mailSender = new JavaMailSenderImpl();
                    //外部加载数据
                    FileReader fileReader = new FileReader("./pass/mailInfo.txt");
                    BufferedReader bufferedReader = new BufferedReader(fileReader);
                    emailFrom = bufferedReader.readLine();
                    password = bufferedReader.readLine();

                    mailSender.setHost(host);//指定用来发送Email的邮件服务器主机名
                    mailSender.setPort(SMTPPort);//默认端口，标准的SMTP端口
                    mailSender.setUsername(emailFrom);//用户名
                    mailSender.setPassword(password);//就是刚刚设置的授权码

                    //加认证机制，服务器会封禁25端口
                    Properties javaMailProperties = new Properties();
                    javaMailProperties.put("mail.smtp.auth", true);
                    javaMailProperties.put("mail.smtp.starttls.enable", true);
                    javaMailProperties.put("mail.smtp.starttls.required", true);
                    javaMailProperties.put("mail.smtp.timeout", 5000);
                    javaMailProperties.put("mail.smtp.ssl.enable",true);
                    javaMailProperties.put("mail.smtp.socketFactory.class",javax.net.ssl.SSLSocketFactory.class);

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
        helper.setSubject("【MIAO】 验证码");//主题
        helper.setText("尊敬的用户：\n    您好！\n    欢迎您加入MIAO大家庭，您的验证码是 " + code +
                "。此验证码5分钟内有效。\n                                                         " +
                "MIAO 敬上", false);

        //发送邮件
        javaMailSender.send(message);
    }

}

```

# 4. 参考
1. <a href = "https://blog.csdn.net/qq_22465297/article/details/84035252?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522159391576619195265917851%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=159391576619195265917851&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v3~pc_rank_v2-4-84035252.first_rank_ecpm_v3_pc_rank_v2&utm_term=Spring%E5%A6%82%E4%BD%95%E5%8F%91%E9%80%81%E9%82%AE%E4%BB%B6">SSM 框架整合 spring 发送邮件功能实现！</a>