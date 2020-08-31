Flask 响应
---
1. Flask支持两种响应方式
2. Flask支持自定义HTTP响应状态码

# 1. 响应HTML文本
```py
@app.route("/")
def index():
    # [默认支持]响应html文本
    return "<img src='http://flask.pocoo.org/static/logo.png'>"
```

# 2. 响应JSON数据
```py
from flask import Flask, request, jsonify

@app.route("/")
def index():
    # 也可以响应json格式代码
    data = [
        {"id":1,"username":"liulaoshi","age":18},
        {"id":2,"username":"liulaoshi","age":17},
        {"id":3,"username":"liulaoshi","age":16},
        {"id":4,"username":"liulaoshi","age":15},
    ]
    return jsonify(data)
```

# 3. 响应重定向

## 3.1. 外部重定向
```py
# 页面跳转响应
from flask import Flask, request, jsonify, redirect
@app.route("/user")
def user():
    # 页面跳转 redirect函数就是response对象的页面跳转的封装
    # Location: http://www.baidu.com
    return redirect("http://www.baidu.com")
```

## 3.2. 内部重定向

### 3.2.1. 内部无参跳转
```py
#使用url_for可以实现视图方法之间的内部跳转
# url_for("视图方法名")
from flask import Flask, request, jsonify, redirect, url_for
@app.route("/login")
def login():
    return redirect( url_for("index") )
```

### 3.2.2. 内部有参跳转
```py
# 路由传递参数
@app.route('/user/<user_id>')
def user_info(user_id):
    return 'hello %s' % user_id

# 重定向
@app.route('/demo4')
def demo4():
    # 使用 url_for 生成指定视图函数所对应的 url
    return redirect(url_for('user_info', user_id=100))
```

# 自定义状态码
```py
@app.route('/demo4')
def demo4():
    return '状态码为 666', 400
```