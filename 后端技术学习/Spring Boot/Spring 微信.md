Spring 微信
---
1. 本文主要记录了Spring Boot和微信的对应服务器进行通信的过程
2. 这部分主要参考了zoutt的实现。

<!-- TOC -->

- [1. WxUtil](#1-wxutil)
- [2. WeiXinCommonUtil](#2-weixincommonutil)
- [3. 其他辅助类](#3-其他辅助类)
  - [3.1. HttpRequestUtil](#31-httprequestutil)
  - [3.2. StringUtil](#32-stringutil)
  - [3.3. MyX509TrustManager](#33-myx509trustmanager)

<!-- /TOC -->

# 1. WxUtil
```java
@Component
public class WxUtil {
    private static String appid = "";
    private static String appSecret = "";

    @Autowired
    WeiXinCommonUtil weiXinCommonUtil;

    private void init(){
        try{
            FileReader fileReader = new FileReader("filename");
            BufferedReader bufferedReader = new BufferedReader(fileReader);
            String[] str = bufferedReader.readLine().split(" ");
            appid = str[0];
            appSecret = str[1];
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 服务器获取AccessToken,第一次延迟1秒执行，当执行完后7000秒再执行
     */
    @Scheduled(initialDelay = 1000, fixedDelay = 7000*1000)
    public void getWeiXinAccessToken(){
        if(appid.length() == 0 || appSecret.length() == 0){
            init();
        }
        try {
            AccessToken token = null;
            int cnt = 0;
            do{
                token = weiXinCommonUtil.getToken(appid, appSecret);
                cnt++;
            }while (token == null && cnt < 2);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 用户登录,并完成加密
     * @param js_code
     * @return
     */
    public UserLoginVO getWeiXinLogin(String js_code){
        if(appid.length() == 0 || appSecret.length() == 0){
            init();
        }
        UserLoginVO userLoginVO = null;
        try{
            userLoginVO = null;
            int cnt = 0;
            do{
                userLoginVO = weiXinCommonUtil.getUserLogin(appid,appSecret,js_code);
                cnt ++;
            }while (userLoginVO == null && cnt <2);
        }catch (Exception e){
            e.printStackTrace();
        }
        String open_id_md5 = MD5Encryption.encrypt(userLoginVO.getOpen_id());
        if(open_id_md5 == null){
            System.out.println("open_id加密失败");
            return null;
        }
        System.out.println("open_id_5:"+ open_id_md5);
        userLoginVO.setOpen_id(open_id_md5);
        return userLoginVO;
    }

}
```

# 2. WeiXinCommonUtil
```java
@Component
public class WeiXinCommonUtil {

    @Autowired
    private HttpRequestUtil httpRequestUtil;

    // 获取access_token的接口地址（GET） 限2000（次/天）
    private static String accessTokenUrl = "https://api.weixin.qq.com/cgi-bin/token?" +
            "grant_type=client_credential&appid=APPID&secret=APPSECRET";
    private static String code2SessionUrl= "https://api.weixin.qq.com/sns/jscode2session?" +
            "appid=APPID&secret=SECRET&js_code=JSCODE&grant_type=authorization_code";

    /**
     * 获取AccessToken
     * @param appid
     * @param appsecret
     * @return
     */
    public AccessToken getToken(String appid, String appsecret){
        AccessToken token=null;
        //访问微信服务器的地址
        String requestUrl=accessTokenUrl.replace("APPID", appid).replace("APPSECRET", appsecret);
        //创建一个json对象
        JSONObject json =httpRequestUtil.httpsRequest(requestUrl,"GET",null);
        System.out.println("获取到的json格式的Token为:"+json);
        //判断json是否为空
        if (json!=null){
            try{
                token = new AccessToken();
                token.setAccess_token(json.getString("access_token"));
                token.setExpires_in(json.getInt("expires_in"));
            }
            catch (Exception e){
                token=null;
                e.printStackTrace();
                System.out.println("系统异常！");
            }
        }else {
            token=null;
            System.out.println("Fail:" + json);
        }
        return token;
    }

    /**
     * 获取登录信息
     * @param appid
     * @param appsecret
     * @param js_code
     * @return
     */
    public UserLoginVO getUserLogin(String appid, String appsecret, String js_code){
        UserLoginVO userLoginVO = null;
        //访问微信服务器的地址
        String requestUrl = code2SessionUrl
                .replace("APPID", appid)
                .replace("SECRET", appsecret)
                .replace("JSCODE",js_code);
        //创建一个json对象
        JSONObject json =httpRequestUtil.httpsRequest(requestUrl,"GET",null);
        System.out.println("获取到的json格式的Token为:"+json);
        //判断json是否为空
        if (json!=null){
            try{
                userLoginVO = new UserLoginVO();
                userLoginVO.setOpen_id(json.getString("openid"));
                userLoginVO.setSession_key(json.getString("session_key"));
            }catch (Exception e){
                userLoginVO = null;
                e.printStackTrace();
                System.out.println("系统异常！");
            }
        }else {
            userLoginVO = null;
            System.out.println("Fail:" + json);
        }
        return userLoginVO;
    }
}
```

# 3. 其他辅助类

## 3.1. HttpRequestUtil
```java
@Component
public class HttpRequestUtil {
    /**
     * 发起https请求并获取结果
     *
     * @param requestUrl    请求地址
     * @param requestMethod 请求方式（GET、POST）
     * @param outputStr     提交的数据
     * @return JSONObject(通过JSONObject.get  (key)的方式获取json对象的属性值)
     */
    public static JSONObject httpsRequest(String requestUrl, String requestMethod, String outputStr) {
        JSONObject jsonObject = null;
        //读取到缓冲区
        StringBuffer buffer = new StringBuffer();
        try {
            // 创建SSLContext对象，并使用我们指定的信任管理器初始化
            TrustManager[] tm = {new MyX509TrustManager()};
            SSLContext sslContext = SSLContext.getInstance("SSL", "SunJSSE");
            sslContext.init(null, tm, new java.security.SecureRandom());
            // 从上述SSLContext对象中得到SSLSocketFactory对象
            SSLSocketFactory ssf = sslContext.getSocketFactory();

            URL url = new URL(requestUrl);
            HttpsURLConnection httpUrlConn = (HttpsURLConnection) url.openConnection();
            httpUrlConn.setSSLSocketFactory(ssf);

            httpUrlConn.setDoOutput(true);
            httpUrlConn.setDoInput(true);
            httpUrlConn.setUseCaches(false);
            // 设置请求方式（GET/POST）
            httpUrlConn.setRequestMethod(requestMethod);

            if ("GET".equalsIgnoreCase(requestMethod)) httpUrlConn.connect();

            // 当有数据需要提交时
            if (!StringUtil.isEmpty(outputStr)) {
                OutputStream outputStream = httpUrlConn.getOutputStream();
                // 注意编码格式，防止中文乱码
                outputStream.write(outputStr.getBytes("UTF-8"));
                outputStream.close();
            }

            // 将返回的输入流转换成字符串
            InputStream inputStream = httpUrlConn.getInputStream();
            InputStreamReader inputStreamReader = new InputStreamReader(inputStream, "utf-8");
            BufferedReader bufferedReader = new BufferedReader(inputStreamReader);

            String str = null;
            while ((str = bufferedReader.readLine()) != null) {
                buffer.append(str);
            }
            bufferedReader.close();
            inputStreamReader.close();
            // 释放资源
            inputStream.close();
            inputStream = null;
            httpUrlConn.disconnect();
            jsonObject = JSONObject.fromObject(buffer.toString());
        } catch (ConnectException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return jsonObject;

    }
}
```

## 3.2. StringUtil
```java
public class StringUtil {
    public static boolean isEmpty(String str) {
        return str == null || str.trim().length() == 0;
    }
}
```

## 3.3. MyX509TrustManager
```java
/**
 * 信任管理器
 *
 * @author zoutt
 * @date 2018-12-19
 */
public class MyX509TrustManager implements X509TrustManager {

    // 检查客户端证书
    public void checkClientTrusted(X509Certificate[] chain, String authType) throws CertificateException {
    }

    // 检查服务器端证书
    public void checkServerTrusted(X509Certificate[] chain, String authType) throws CertificateException {
    }

    // 返回受信任的X509证书数组
    public X509Certificate[] getAcceptedIssuers() {
        return null;
    }
}
```