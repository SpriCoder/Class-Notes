Flask 路由
---

# 1. 不含参的路由

## 1.1. 一般形式
```py
# 指定访问路径为 demo
@app.route('/demo')
def demo():
    return 'demo'
```

## 1.2. 限定请求方式
```py
from flask import Flask, request
@app.route('/demo', methods=['GET', 'POST'])
def demo():
  # 直接从请求中取到请求方式并返回
  return request.method
```

# 2. 含参的路由

## 2.1. 不限定类型的路由
```py
# 路由传递参数[没有限定类型]
@app.route('/user/<user_id>')
def user_info(user_id):
    return 'hello %s' % user_id
```

## 2.2. 限定类型的路由
```py
# 路由传递参数[限定数据类型]
@app.route('/user/<int:user_id>')
def user_info(user_id):
    return 'hello %d' % user_id
```
## 2.3. 正则匹配路由
1. 限制用户访问控制的时候往往需要用到正则匹配路由
2. 步骤：
  1. 导入转换器基类:所有的路由器匹配规则都应该使用转换器对象进行记录
  2. 自定义转换器：自定义类继承于转换器基类
  3. 添加转换器到默认的转换器字典中
  4. 使用自定义转换器实现自定义匹配规则
3. 转换基类:`from werkzeug.routing import BaseConverter`

### 系统自带转换器
```py
DEFAULT_CONVERTERS = {
    'default':          UnicodeConverter,
    'path':             PathConverter,
    'string':           UnicodeConverter,
    'any':              AnyConverter,
    'int':              IntegerConverter,
    'float':            FloatConverter,
    'uuid':             UUIDConverter,
}
```

### 2.3.1. 自定义转换器
```py
# 自定义正则转换器
from werkzeug.routing import BaseConverter
class RegexConverter(BaseConverter):
    def __init__(self,url_map,*args):
        super(RegexConverter, self).__init__(url_map)
        # 正则参数
        self.regex = args[0]
```

### 2.3.2. 添加转换器到默认的转换器字典
1. 指定转换器使用名字为re
```py
# 将自定义转换器添加到转换器字典中，并指定转换器使用时名字为: re
app.url_map.converters['re'] = RegexConverter
```

### 2.3.3. 使用转换器来实现自定义匹配规则:手机号码
```py
# 正则匹配路由
@app.route("/login/<re('1\d{10}'):mobile>")
def login(mobile):
    return mobile
```

### 如上完整的代码如下
```py
rom flask import Flask
# 新增一个配置文件,在配置文件中设置配置信息
from config import Config
from flask import request
# from collections import OrderedDict  # 有序字典

app = Flask(__name__)

# 调用app.config加载配置
app.config.from_object(Config)


from werkzeug.routing import BaseConverter

class RegexConverter(BaseConverter):
    def __init__(self,url_map,*args):
        super(RegexConverter, self).__init__(url_map)
        # 正则参数
        self.regex = args[0]

# converter["路由转换器名称"] = 实现路由转换功能的自定义类
app.url_map.converters['re'] = RegexConverter
# 绑定路由
@app.route("/")
def index():
    return "hello flask"


# 默认情况下,路由使用的就是关键字参数,也叫"命名路由"
# router(路由地址, http请求方式 )
@app.route("/list/<int:page>/<string:content>",methods=["GET","POST"])
def mylist(content,page):
    return "第%s页<br>内容:%s" % (page,content)

# 正则匹配路由
@app.route("/login/<re('1\d{10}'):mobile>")
def login(mobile):
    return mobile
```