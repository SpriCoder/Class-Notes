Spring与OSS
---
1. 借助Spring向OSS后台上传图片、视频等文件数据是很重要的步骤，本文记录了简单的OSS上传方式

<!-- TOC -->

- [1. OSS配置方式一:](#1-oss配置方式一)
  - [1.1. 首先在yml文件中写死](#11-首先在yml文件中写死)
  - [1.2. 创建配置类进行配置](#12-创建配置类进行配置)
  - [1.3. 创建Util类](#13-创建util类)
- [2. 完整的上传OSS文件](#2-完整的上传oss文件)
  - [2.1. pom.xml文件的配置](#21-pomxml文件的配置)
  - [2.2. yml文件中的配置](#22-yml文件中的配置)
  - [2.3. Controller组件](#23-controller组件)
  - [2.4. Service组件](#24-service组件)
  - [2.5. OSSClientUtil配置类](#25-ossclientutil配置类)

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

# 2. 完整的上传OSS文件

## 2.1. pom.xml文件的配置
```xml
<dependency>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-sdk-oss</artifactId>
    <version>3.9.1</version>
</dependency>
```

## 2.2. yml文件中的配置
1. 主要是可以调整上传文件的最大大小
```yml
spring:
    servlet:
        multipart:
            max-file-size : 10MB
            max-request-size : 100MB
```

## 2.3. Controller组件
```java
@RestController
@RequestMapping("/oss")
public class UploadController {

    @Autowired
    UploadService uploadService;

    /**
     * 上传文件
     * @param multipartFile
     * @return
     */
    @PostMapping("/upload")
    public SimpleResponse upload(@RequestBody MultipartFile multipartFile){
        return uploadService.upload(multipartFile);
    }

    /**
     * 删除文件
     * @param url url为全部文件名（无前缀）xxx.jpg
     * @return
     */
    @PostMapping("/delete")
    public SimpleResponse delete(@RequestParam String url){
        return uploadService.delete(url);
    }
}
```

## 2.4. Service组件
```java
@Service
public class UploadService {
    @Autowired
    OSSClientUtil ossClientUtil;

    public SimpleResponse upload(MultipartFile multipartFile){
        if(multipartFile == null){
            return SimpleResponse.error("上传文件问题");
        }
        if(multipartFile.getSize() <= 0){
            return SimpleResponse.error("文件大小有误");
        }
        if(multipartFile.getSize() > 10 * 1024 * 1024){
            return SimpleResponse.error("文件大小不能大于10M");
        }
        String url;
        try{
            url = ossClientUtil.uploadFile(multipartFile);
        }catch (Exception e){
            e.printStackTrace();
            return SimpleResponse.exception("上传失败");
        }
        return SimpleResponse.ok(url);
    }

    public SimpleResponse delete(String url){
        try{
            ossClientUtil.delete(url);
        }catch (Exception e){
            e.printStackTrace();
            return SimpleResponse.error("删除失败");
        }

        return SimpleResponse.ok("删除成功");
    }

}
```

## 2.5. OSSClientUtil配置类
```java
@Component
public class OSSClientUtil {
    Log log = LogFactory.getLog(OSSClientUtil.class);

    //阿里云OSS地址，这里看根据你的oss选择
    protected static String endpoint = "http://oss-cn-shanghai.aliyuncs.com";
    //阿里云OSS账号
    protected static String accessKeyId = "";
    //阿里云OSS密钥
    protected static String accessKeySecret = "";
    //阿里云OSS上的存储块bucket名字
    protected static String bucketName = "xxx";

    private static String filename = "";

    private volatile static OSS ossClient = null;//不进行序列化

    public String uploadFile(MultipartFile file) throws Exception {
        initOSS();
        String[] fileWords = file.getContentType().split("/");
        String time = TimeUtil.dateToString(LocalDateTime.now());
        String filename = time + "/" + System.currentTimeMillis()
                + "-" + UUID.randomUUID().toString().substring(0, 18)
                + "." + fileWords[fileWords.length - 1];
        System.out.println(filename);
        try {
            ossClient.putObject(new PutObjectRequest(bucketName, filename , file.getInputStream()));
        } catch (OSSException oe) {
            System.out.println(TimeUtil.preTime() + "Caught an OSSException, which means your request made it to OSS, "
                    + "but was rejected with an error response for some reason.");
            System.out.println(TimeUtil.preTime() + "Error Message: " + oe.getErrorMessage());
            System.out.println(TimeUtil.preTime() + "Error Code:       " + oe.getErrorCode());
            System.out.println(TimeUtil.preTime() + "Request ID:      " + oe.getRequestId());
            System.out.println(TimeUtil.preTime() + "Host ID:           " + oe.getHostId());
            throw oe;
        } catch (ClientException ce) {
            System.out.println(TimeUtil.preTime() + "Caught an ClientException, which means the client encountered "
                    + "a serious internal problem while trying to communicate with OSS, "
                    + "such as not being able to access the network.");
            System.out.println(TimeUtil.preTime() + "Error Message: " + ce.getMessage());
            throw ce;
        }
        return "https://" + bucketName + "." + endpoint.split("//")[1] + "/" + filename;
    }

    public boolean delete(String url) throws Exception{
        //读入对应密码
        initOSS();
        try {
            ossClient.deleteObject(bucketName,url);
        } catch (OSSException oe) {
            System.out.println(TimeUtil.preTime() + "Caught an OSSException, which means your request made it to OSS, "
                    + "but was rejected with an error response for some reason.");
            System.out.println(TimeUtil.preTime() + "Error Message: " + oe.getErrorMessage());
            System.out.println(TimeUtil.preTime() + "Error Code:       " + oe.getErrorCode());
            System.out.println(TimeUtil.preTime() + "Request ID:      " + oe.getRequestId());
            System.out.println(TimeUtil.preTime() + "Host ID:           " + oe.getHostId());
            throw oe;
        } catch (ClientException ce) {
            System.out.println(TimeUtil.preTime() + "Caught an ClientException, which means the client encountered "
                    + "a serious internal problem while trying to communicate with OSS, "
                    + "such as not being able to access the network.");
            System.out.println(TimeUtil.preTime() + "Error Message: " + ce.getMessage());
            throw ce;
        }
        return true;
    }

    private static OSS initOSS() throws Exception
    {
        if(ossClient == null)
        {
            synchronized (OSSClientUtil.class)
            {
                if(ossClient == null)
                {
                    try{
                        FileReader fileReader = new FileReader("./pass/pass.txt");
                        BufferedReader bufferedReader = new BufferedReader(fileReader);
                        String[] str = bufferedReader.readLine().split(" ");
                        accessKeyId = str[0];
                        accessKeySecret = str[1];
                    }catch (Exception e){
                        e.printStackTrace();
                        throw e;
                    }
                    ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
                }
            }
        }
        return ossClient;
    }

}
```