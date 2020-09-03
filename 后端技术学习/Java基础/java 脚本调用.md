java-脚本调用
---

# 1. 使用Runtime对python进行传参
```java
package test;
import java.io.BufferedReader;
import java.io.InputStreamReader;
public class MyDemo {
    public static void main(String[] args) { 
        try {
           System.out.println("start");
           String[] args1=new String[]{"python","D:\\pyworkpeace\\9_30_1.py","D:\\basic\\data.txt"};//参数列表，python的sys.argv获得到的参数组

           Process pr=Runtime.getRuntime().exec(args1);
            
           BufferedReader in = new BufferedReader(new InputStreamReader(
             pr.getInputStream()));
           String line;
           while ((line = in.readLine()) != null) {
            System.out.println(line);
           }
           in.close();
           pr.waitFor();
           System.out.println("end");
          } catch (Exception e) {
           e.printStackTrace();
          }
    }
    public void test(){
        System.out.println("我的第一个方法C");
    }
 
}
```

# 2. 参考
1. <a href = "https://www.cnblogs.com/bethansy/p/7614749.html">java调用python脚本并向python脚本传递数据实现</a>
2. <a href = "https://blog.csdn.net/qq_26591517/article/details/80441540">Java调用Python程序方法总结(最全最详细)</a>