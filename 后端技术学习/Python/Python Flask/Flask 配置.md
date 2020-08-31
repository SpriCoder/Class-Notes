Flask 配置
---

# 1. 配置类
```py
class Config(object):
    DEBUG = True
    SECRET_KEY = "abcccddgadsag"

app.config.from_object(Config)
```

# 2. 指定服务器IP和端口
```py
app.run(host="0.0.0.0", port=5000, debug = True)
```