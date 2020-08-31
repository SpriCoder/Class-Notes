Flask 基础
---
1. 在做项目的过程中，机缘巧合下遇到了Flask，也就顺手准备开始写相关部分的笔记。

# 1. Flask安装
```py
pip install flask
```

# 2. 最简单的Flask-Hello World
```py
from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello World'
    
if __name__ == '__main__':
    app.run()
```

# 3. 参考
1. <a href = "https://www.cnblogs.com/Sunzz/p/10956837.html">Flask入门很轻松 （一）</a>