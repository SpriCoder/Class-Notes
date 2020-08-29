Flask 部署
---
1. Flask在部署到服务器的时候，需要配置类似Spring Boot的Tomcat的环境
2. 我们选择使用`gunicorn`来完成部署

# 1. 配置环境
```py
pip install gunicorn
pip install gevent
```

1. 之后可以根据自己的需要修改gunicorn的配置文件：`gunicorn.conf.py`，比如如下
```py
workers = 2    # 定义同时开启的处理请求的进程数量，根据网站流量适当调整
worker_class = "gevent"   # 采用gevent库，支持异步处理请求，提高吞吐量
bind = "0.0.0.0:7999"
```

# 2. 运行Flask
1. 使用如下命令即可运行:`gunicorn -D YourFileName:app -c gunicorn.conf.py`