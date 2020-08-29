Spring Python
---
1. 在使用Spring Boot的过程中，难免会遇到需要调用Python的问题，这时候我们有多种选择

# 1. 选择JPython
1. 是一种方法，但是当调用比较复杂的时候会有一定的问题
2. <a href = "https://www.jianshu.com/p/cefdaccd3fd3">jpython的使用(Java调用python脚本)</a>

## 1.1. pom配置
```xml
<dependency>
    <groupId>org.python</groupId>
    <artifactId>jython-standalone</artifactId>
    <version>2.7.0</version>
</dependency>
```

## 1.2. 直接执行Python
```java
import org.python.util.PythonInterpreter;
public class FirstJavaScript {
    public static void main(String args[]) {
        //环境配置部分
        Properties props = new Properties();
        props.put("python.home", "path to the Lib folder");
        props.put("python.console.encoding", "UTF-8");
        props.put("python.security.respectJavaAccessibility", "false");
        props.put("python.import.site", "false");
        Properties preprops = System.getProperties();
        PythonInterpreter.initialize(preprops, props, new String[0]);

        //执行部分
        PythonInterpreter interpreter = new PythonInterpreter();
        interpreter.exec("days=('mod','Tue','Wed','Thu','Fri','Sat','Sun'); ");
        interpreter.exec("print days[1];");
    }// main
}
```

## 1.3. 执行Python文件中的方法
1. Python文件

```py
# test.py
def add(a, b):
    return a + b
```

2. 执行Python文件中的方法:
```java
Properties props = new Properties();
props.put("python.home", "path to the Lib folder");
props.put("python.console.encoding", "UTF-8");
props.put("python.security.respectJavaAccessibility", "false");
props.put("python.import.site", "false");
Properties preprops = System.getProperties();
PythonInterpreter.initialize(preprops, props, new String[0]);

PythonInterpreter interpreter = new PythonInterpreter();
interpreter.execfile("test.py");
PyFunction func = (PyFunction) interpreter.get("add",
    PyFunction.class);
int a = 100, b = 100;
PyObject pyobj = func.__call__(new PyInteger(a), new PyInteger(b));
System.out.println("anwser = " + pyobj.toString());
```

## 1.4. 执行Python文件中的主方法
1. Python文件

```py
# -*- coding: utf-8 -*
# test.py
print ("hello")
ls = [1,2,3,4,5,6]
print(ls)
print('你好')
```

2. 使用Java调用Python执行主方法
```java
Properties props = new Properties();
props.put("python.home", "path to the Lib folder");
props.put("python.console.encoding", "UTF-8");
props.put("python.security.respectJavaAccessibility", "false");
props.put("python.import.site", "false");
Properties preprops = System.getProperties();
PythonInterpreter.initialize(preprops, props, new String[0]);

PythonInterpreter interpreter = new PythonInterpreter();
interpreter.execfile("test.py");
```

# 2. 选择发起网络请求
1. 就是通过改写你的Python端，使其运行在例如Flask的框架下，发起请求即可。
2. 关键的Java代码如下(本代码应用在发送文件的情况下)

```java
String OtherServerPath = "http://localhost:7999/pic";

Map<String, String> textMap = new HashMap<String, String>();
//设置file的name，路径
Map<String, String> fileMap = new HashMap<String, String>();
fileMap.put("upload", fileName);
String contentType = "";//image/png
String ret = formUpload(OtherServerPath, textMap, fileMap, contentType);

//转换
JSONObject jsonObject = JSONObject.fromString(ret);
Iterator myNames = jsonObject.keys();
while(myNames.hasNext()){
  String name = (String)myNames.next();
  JSONObject inner = (JSONObject) jsonObject.get(name);
  JSONArray innerBox = (JSONArray) inner.get("boxes");
  Double[] box = {innerBox.getDouble(0), innerBox.getDouble(1),
    innerBox.getDouble(2), innerBox.getDouble(3)};
  RecognizeResult recognizeResult = new RecognizeResult(name, box, (Double)inner.get("similarity"));
  System.out.println(recognizeResult);
  recognizeResults.add(recognizeResult);
}
```

3. 与之相对应的Python代码如下
```python
# -*- coding: utf-8 -*-
from flask import Flask, request, jsonify
import yolo_image
import os

basedir = os.path.abspath(os.path.dirname(__file__))
app = Flask(__name__)

# 支持GET请求和POST请求
@app.route('/pic', methods=['GET','POST'])
def yolo():
    # 获取图片文件 name = upload
    img = request.files.get('upload')
    if(img == None):
        print("Failed to open")
        return jsonify([])
    # 定义一个图片存放的位置 存放在static下面
    path = basedir + "/static/img/"
    # 图片名称
    imgName = img.filename

    # 图片path和名称组成图片的保存路径
    picture = path + imgName

    # 保存图片
    img.save(picture)

    print(picture)
    result = yolo_image.main(picture)
    if(result == None):
        return "Not Find!"

    datas = {}
    for data in result:
        if(data[0] in datas.keys()):
            if(float(data[2]) < datas[data[0]]['similarity']):
                continue
        datas[data[0]] = {}
        datas[data[0]]['boxes'] = list(map(float,data[1]))
        datas[data[0]]['similarity'] = float(data[2])

    os.remove(picture)
    return jsonify(datas)

if __name__ == '__main__':
    # 真正运行环境下不要使用debug运行
    app.run(host="0.0.0.0", port=7999, debug=True)
```