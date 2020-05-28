---
title: 笔记(使用新浪微博实现三方登录)
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


<h2>使用微博进行第三方登录

<h4>首先需要注册微博账号


###进入到新浪微博开放平台
https://open.weibo.com/index.php



<div align="right"><img src='http://q9hs8ny3b.bkt.clouddn.com/%5B0_%5DVOPJAZOOS%60%249IT%40D%2812.gif' type='img/gif'></div>

<h2>在做三方登录之前可以通过画思维导图使自己有更好的思路进行一下的操作

![weibo6](/hexo-personage/images/weibo6.png)

再进行以下操作


填写名称，再点击创建
![weibo1](/hexo-personage/images/weibo1.png)



找到我的应用，下面会出现应用信息，找到基本信息
![weibo2](/hexo-personage/images/weibo2.png)


然后找到高级信息，填写这两个值，可以是不一样的
![weibo3](/hexo-personage/images/weibo3.png)


找到测试信息，可以通过编辑添加微博好友，添加好友是为了，多个微博进行登录
![weibo4](/hexo-personage/images/weibo4.png)


以上都是准备工作，只是简单的建立一个微应用


在登录的前提是必须退出微博登录状态，不然就不会进行登录操作

![weibo5](/hexo-personage/images/weibo5.png)

现在到django中写新浪微博登录的接口

###后端代码
```python

#导包操作
import requests
import json

#新浪微博回调
def wb_back(request):
    #获取code
    code = request.GET.get('code')
    url = "https://api.weibo.com/oauth2/access_token"

    #参数
    res = requests.post(
        url,
    data ={
        'client_id':"4217859619",   #App Key：4217859619
        "client_secret":"4490894bb47ca4e00a3859bf92eb473a",   #App Secret：4490894bb47ca4e00a3859bf92eb473a
        "grant_type":"authorization_code",
        "code":code,
        "redirect_uri":"http://127.0.0.1:8000/md_admin/weibo"
    })
    #转换类型
    res = json.loads(res.text)


    #获取新浪微博的昵称
    result = requests.get('https://api.weibo.com/2/users/show.json',params={
        'access_token':res['access_token'],'uid':res['uid']
    })

    result = json.loads(result.text)

    #判断该用户是否曾经登录过
    user = User.objects.filter(username=str(result['name'])).first()

    sina_id = ''
    user_id = ''

    if user:
        sina_id = user.username
        user_id = user.id
    else:
        #手动创建账号
        user = User(username=str(result['name']),password='')
        user.save()

        sina_id = result['name']

        #到数据库中查询一下刚入库的新账号id
        user = User.objects.filter(username=str(result['name'])).first()
        user_id = user.id
    return redirect("http://localhost:8080?sina_id="+sina_id+"&uid="+str(
        user_id))
    # return HttpResponse(result['name'])

```

###前端代码
```vue

<img @click="sina" style="cursor: pointer" src="http://127.0.0.1:8000/static/sina.png">

methons{
 //新浪微博登录接口
      sina:function(){

          //拼接url
          let clinet_id = 4217859619;

          let url = "https://api.weibo.com/oauth2/authorize?client_id=4217859619&redirect_uri=http://127.0.0.1:8000/md_admin/weibo";


          //跳转
          window.location.href = url;

      },
}


```


最后登录成功的鸭子
![weibo7](/hexo-personage/images/weibo7.png)