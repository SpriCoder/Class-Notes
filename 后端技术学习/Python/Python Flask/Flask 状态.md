Flask 状态
---
1. HTTP本身是无状态协议，可以通过Cookie和Session的方式来解决状态维持的问题。

# 1. Cookie
1. 浏览器在请求某个网页的时候，会将本网站下的所有的Cookie信息提交给服务器，所以request中可以读取cookie的信息
2. Cookie是存储在浏览器的纯文本信息，建议不要存储敏感信息如密码
  
## 1.1. 设置Cookie
```py
from flask imoprt Flask,make_response
@app.route('/set_cookie')
def set_cookie():
    resp = make_response('this is to set cookie')
    resp.set_cookie('username', 'xiaoming', max_age=3600)
    # max_age 为cookie有效期，单位为秒
    return resp
```

## 1.2. 获取cookie
```py
from flask import Flask,request
@app.route('/get_cookie')
def resp_cookie():
    resp = request.cookies.get('username')
    return resp
```

# 2. Session
1. Session是存储在服务器端进行状态保持的
2. Session依赖于Cookie

## 2.1. 设置Session
```py
from flask import session
@app.route('/set_session')
def set_session():
    session['username'] = 'xiaoming'
    return 'ok!'
```

## 2.2. 获取session
```py
@app.route('/get_session')
def get_session():
    return session.get('username')
```