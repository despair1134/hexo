---
title: 笔记(注册又拍云以及实现上传文件操作)
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


###准备工作，需要安装upyun这个插件 pip install upyun


<h2>需要注册又拍云账号,然后进行以下操作

![upyun_demo1](/hexo-personage/images/upyun_demo1.png)

![upyun_demo3](/hexo-personage/images/upyun_demo3.png)

![upyun_demo2](/hexo-personage/images/upyun_demo2.png)




```python
from rest_framework.response import Response
from rest_framework.views import APIView
import upyun


# 定义又拍云文件上传类
class UpYun(APIView):

    def post(self, request):
        file = request.FILES.get('file')
        print(file)

        up = upyun.UpYun('images-upyun1', 'text123', 'jyuKWc0RWZ9OZlXr3KGn4D8Xr3nu5ka4')
        
        #images-upyun1是你的服务名称，text123是你的授权时的操作员名称，jyuKWc0RWZ9OZlXr3KGn4D8Xr3nu5ka4是你一开始创建服务时的那个密码

        headers = {'x-gmkerl-rotate': 'auto'}

        for chunk in file.chunks():

            res = up.put('/demoop.png', chunk, checksum=True, headers=headers)

        # 返回结果
        return Response({'filename':file.name})

#     http: // text123.test.upcdn.net /
```


###vue页面的代码实现
```vue

<template>


	<div>
		<img :src="src" >
		<Avatar :src="src" :width='150' fit='fill'></Avatar>

		<input type="file" @change='upload_upyun'>
		<div id='upload'>
			拖拽上传
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			src:'',
      dirname:'',
		}
	},
	//监听属性
	watch:{

	},
	//计算属性
	computed: {

	},
	//钩子方法
	mounted() {
		// 注册拖拽容器
		let upload = document.querySelector('#upload')
		// 声明监听事件 不声明就是普通div
		upload.addEventListener('dragenter',this.onDrag,false)
		upload.addEventListener('dragover',this.onDrag,false)
		upload.addEventListener('drop',this.onDrop,false)
	},
	//自定义方法
	methods: {
		// 监听用户鼠标动作

		onDrag (e) {
			e.stopPropagation();
			e.preventDefault();
    	},
		onDrop (e) {
		e.stopPropagation();
		e.preventDefault();
		// 调用自定义上传方法
		this.upload_upyun(e.dataTransfer.files);
		},

		//上传又拍云
		upload_upyun:function(files){


		//获取文件对象
		//let file = e.target.files[0];
		let file = files[0];

		//声明参数
		let param = new FormData();
		param.append('file',file);

		// 声明头部信息
		const config = {

			headers: { 'Content-Type': 'multipart/form-data' }

		}
		// 上传图片
		this.axios.post('http://localhost:8000/upyun/', param, config).then(function(response){

			console.log(response)

		}).then(result=>{

			// 赋值src展示图片，需要到又拍云图片管理上获取图片网址，然后动态拼接
			this.src = 'http://images-upyun1.test.upcdn.net/demoop.png'
		})

		}

}
}
</script>

<style>
/* 类选择器. */
.upload {
  margin: 100px auto;
  width: 300px;
  height: 150px;
  border: 2px dashed #f00;
}

/* id选择器唯一 # */
#upload{
  margin: 100px auto;
  width: 300px;
  height: 150px;
  border: 2px dashed #f00;
}
</style>


```


##需要注意的是后端是将图片写死的，所以只能上传一张图片，最终实现结果如图所示

![upyun_demo4](/hexo-personage/images/upyun_demo4.png)






