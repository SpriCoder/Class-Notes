Spring与OSS
---
1. 借助Spring向OSS后台上传图片、视频等文件数据是很重要的步骤，本文记录了简单的OSS上传方式

<!-- TOC -->

- [1. OSS配置方式一:](#1-oss配置方式一)
  - [1.1. 首先在yml文件中写死](#11-首先在yml文件中写死)
  - [1.2. 创建配置类进行配置](#12-创建配置类进行配置)
  - [1.3. 创建Util类](#13-创建util类)

<!-- /TOC -->

# 1. OSS配置方式一:

## 1.1. 首先在yml文件中写死
```yml
oss:
  endpoint: oss-cn-hangzhou.aliyuncs.com # OSS服务开通地
  url: https://xxx.oss-cn-hangzhou.aliyuncs.com/ # xxx替换成具体的bucketName
  accessKeyId: xxx # 有权限操作的一个账户，最好是有全部权限，不然会被deny
  accessKeySecret: xxx # 和上面配套的一套密钥
  bucketName: xxx # bucketName
  path: xxx # 存储的文件夹
```

## 1.2. 创建配置类进行配置
```java
@Data
@Configuration
public class OSSConfig {
    @Value("${oss.endpoint}")
    private String endpoint;

    @Value("${oss.url}")
    private String url;

    @Value("${oss.accessKeyId}")
    private String accessKeyId;

    @Value("${oss.accessKeySecret}")
    private String accessKeySecret;

    @Value("${oss.bucketName}")
    private String bucketName;

    @Value("${oss.path}")
    private String path;
}
```

## 1.3. 创建Util类
```java
public class OSSUploadUtil {
    private volatile static OSSClient ossClient = null;//不进行序列化

    public static String upload(OSSConfig ossConfig, MultipartFile file, String fileDir)
    {
        initOSS(ossConfig);
        StringBuilder fileUrl = new StringBuilder();
        try {
            String suffix = file.getOriginalFilename().substring(file.getOriginalFilename().lastIndexOf('.'));
            String fileName = System.currentTimeMillis() + "-" + UUID.randomUUID().toString().substring(0,18) + suffix;//时间 + UUID生成随机的
            if (!fileDir.endsWith("/")) {
                fileDir = fileDir.concat("/");//如果没有则最后加上一个/
            }
            fileUrl = fileUrl.append(fileDir + fileName);

            ossClient.putObject(ossConfig.getBucketName(), fileUrl.toString(), file.getInputStream());
        } catch (IOException e) {
            e.printStackTrace();
            return null;
        }
        fileUrl = fileUrl.insert(0,ossConfig.getUrl());
        return fileUrl.toString();
    }
    //单件模式的一种方式
    private static OSSClient initOSS(OSSConfig ossConfig)
    {
        if(ossClient == null)
        {
            synchronized (OSSUploadUtil.class)
            {
                if(ossClient == null)
                {
                    ossClient = new OSSClient(ossConfig.getEndpoint(),
                            new DefaultCredentialProvider(ossConfig.getAccessKeyId(), ossConfig.getAccessKeySecret()),
                            new ClientConfiguration());
                }
            }
        }
        return ossClient;
    }
}
```