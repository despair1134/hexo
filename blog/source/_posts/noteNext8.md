---
title: 笔记(实现上传头像)
author: despair
avatar: http://127.0.0.1:8000/static/upload/touxiang.jpg
authorLink: despair1134
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
date: 2020-4-5 11:21
comments: true
tags: 
 - web
 - 笔记
keywords: Sakura
description: 笔记
---

<div align="right"><img src='http://q9hs8ny3b.bkt.clouddn.com/%5B0_%5DVOPJAZOOS%60%249IT%40D%2812.gif' type='img/gif'></div>

<h2>准备工作

<h2>思维导图

![upload3](/hexo-personage/images/upload3.png)

<h3>需要建立一个upload文件夹


![upload2](/hexo-personage/images/upload2.png)


```
    # 导入类视图
    from django.views import View
    from mydjango.settings import UPLOAD_ROOT
    import os
    # 导包
    from django.http import HttpResponse, HttpResponseRedirect, JsonResponse
    import json
```

###代码实现django接口，然后配置路由
```python
# 导入类视图
from django.views import View
from mydjango.settings import UPLOAD_ROOT
import os
# 导包
from django.http import HttpResponse, HttpResponseRedirect, JsonResponse
import json

# 定义上传试图类
class UploadFile(View):
    def post(self, request):
        # 接收参数
        img = request.FILES.get("file")

        # 建立文件流对象
        f = open(os.path.join(UPLOAD_ROOT, '', img.name), 'wb')

        # 写入服务器端
        for chunk in img.chunks():
            f.write(chunk)
        f.close()

        # 返回文件名
        return HttpResponse(json.dumps({'filename': img.name}, ensure_ascii=
        False), content_type='application/json')

```



###vue实现代码，最后到index.js中配置

```vue

        <table>
          <tr>

<!--            <div>-->

              <img :src="src" />   //显示原图片
              <Avatar :src="src" :width='150' fit="fill" ></Avatar>   //显示的是平时用的头像大小图片

<!--            </div>-->

            <td>

              上传头像:

            </td>
            <td>

              <input type="file" @change="upload">

            </td>
          </tr>




        </table>


methods:{

      //上传头像提交
		upload:function(e){

			//获取文件对象
			let file = e.target.files[0];

			//声明参数
			let param = new FormData();

			//添加文件
			param.append('file',file,file.name);

			//声明请求头
			let config = {headers:{'Content-Type':'multipart/form-data'}};

			//发送请求
			this.axios.post('http://localhost:8000/updatefile/',param,config).then((result) =>{

							console.log(result);

							//赋值操纵
							this.src = 'http://localhost:8000/static/upload/' + result.data.filename;

			});
      }
```

###实现结果
![upload1](/hexo-personage/images/upload1.png)

