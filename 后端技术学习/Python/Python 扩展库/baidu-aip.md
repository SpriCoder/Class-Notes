baidu-aip
---
1. baidu-aip是百度提供的调用其服务的SDK包

# 1. 调用OCR
1. 首先需要登录百度云平台，在文字识别的应用列表中创建一个应用。
2. 然后编写如下的Python代码

```py
# -*- coding: utf-8 -*-
from aip import AipOcr
import os

#读取图片，path用于传入读取图片的名字
dir = './img'
def read_image(path):
    dir_i = dir + '\\'
    print(dir_i+path)
    with open(dir_i+path, 'rb') as f:
        image = f.read()
    return image

api_key = 'Your_api_key'
app_id='Your_app_id'
secret_key = 'Your_secret_key'
client=AipOcr(app_id,api_key,secret_key)
fs=os.listdir(dir)
file=open(r'output.txt','w',encoding='utf-8')
for image in fs:
    i=read_image(image)
    inf=client.basicGeneral(i)
    for response in inf['words_result']:
        for words in response['words']:
            file.write(words)
        file.write('\n')
    print(inf)
file.close()
```