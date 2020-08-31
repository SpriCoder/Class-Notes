Flask HTTP
---

# request
1. request:flask中代表请求的request对象
2. 作用:在视图函数中取出本次请求数据
3. 包:`from flask import request`

## 常用属性
| 属性    | 说明                     | 类型           |
| ------- | ------------------------ | -------------- |
| data    | 请求的数据，转换为字符串 | *              |
| form    | 记录请求中的表单数据     | MultiDict      |
| args    | 记录请求中的查询参数     | MultiDict      |
| cookies | 记录请求中的cookie信息   | Dict           |
| headers | 记录请求中的请求头       | EnvironHeaders |
| method  | 记录请求使用的HTTP方法   | GET/POST       |
| url     | 记录请求的URL地址        | string         |
| files   | 记录请求上传的文件       | *              |
| json    | 记录请求的json数据       | json           |

## 应用
```py
"""获取客户端的请求"""
@app.route("/request")
def req1():
    # http://127.0.0.1:5000/request?a=1&b=2
    query_string = request.args
    print(query_string) # ImmutableMultiDict([('a', '1'), ('b', '2')])
    
    # http://127.0.0.1:5000/request?username=abcd
    username = request.args.get("username")
    print(username) # abcd

    # http://127.0.0.1:5000/request?username=xiaoming&love=吹牛&love=睡觉
    love = request.args.getlist("love")
    print(love)# ['吹牛', '睡觉']

    # http://127.0.0.1:5000/request?username=xiaoming&love=吹牛&love=睡觉
    # 把传递过来的数据抓换成原生的字典
    data = request.args.to_dict()
    print(data) #  {'username': 'xiaoming', 'love': '吹牛'} 
    return "ok"

@app.route("/request2",methods=["POST"])
def req2():
    """获取post数据和请求头"""
    # 必须传 json 格式的数据
    print( request.data )
    """打印结果:
    b'{\n\t"username":"xiaoming",\n\t"age":18,\n\t"sex":1\n}'
    """
    # 方式一
    import json
    from flask import json
    data = json.loads(request.data)
    print(data)
    """
    {'username': 'xiaoming', 'age': 18, 'sex': 1}
    """
    # 方式二 
    print(request.json)
    """
    {'username': 'xiaoming', 'age': 18, 'sex': 1}
    """
    
    # 获取请求行数据
    print( request.headers )
    print( request.headers.to_list() )

    # 获取上传文件
    print( request.files )
    print( request.files.get("avatar") )
    """
    ImmutableMultiDict([('avatar', <FileStorage: 'avatar.png' ('image/png')>)])
    <FileStorage: 'avatar.png' ('image/png')>
    """

    return "ok"

if __name__ == '__main__':
    app.run()
```