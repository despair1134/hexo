---
title: 笔记(vue+django实现评论功能)
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



```python

import redis
from rest_framework.response import Response
from rest_framework.views import APIView

from myapp.models import Comment
from myapp.myser import CommentSer



# 定义ip地址和端口
host = "127.0.0.1"
port = 6379

# 链接对象
r = redis.Redis(host=host, port=port)

#商品评论获取
class Commentlist(APIView):

    def get(self,request):

        gid = request.GET.get("gid",None)

        # uid = request.GET.get("uid",None)

        #查询数据
        comments = Comment.objects.filter(gid=gid).order_by("-id")

        #序列化
        comments_ser = CommentSer(comments,many=True)

        return Response(comments_ser.data)


#商品评论
class CommentInsert(APIView):

    def post(self,request):

        #获取客户端ip
        if 'HTTP_X_FORWARDED_FOR' in request.META:

            ip = request.META.get('HTTP_X_FORWARDED_FOR')

        else:
            ip = request.META.get('REMOTE_ADDR')


        # if r.get(ip):
        if r.llen(ip) > 3:

            return Response({'code':403,'message':'您评论的过快，请歇一歇'})

        #初始化参数
        comment = CommentSer(data=request.data)

        #验证字段
        if comment.is_valid():

            #进行入库
            comment.save()

            #设置评论时间间隔
            # r.set(ip,"despair1134")
            r.lpush(ip,1)
            r.expire(ip,30)

        return Response({'code':200,'message':'评论成功'})


```

###写好之后配置url


```vue
<!--        商品评论-->

        <textarea v-model="comment"  rows="10" v-autosize v-wordcount="100">


        </textarea>

        <br><br>

        <Button @click="submit" color="blue">提交评论</Button>

        <br>
        <br>

          <ul>

          <li v-for="item in commentlist">

            {{item.uid}} 说:{{item.content}}

          </li>


        </ul>


 //评论内容(在return里面定义好容器)
        comment:'',
        //评论列表
        commentlist:[]

 //获取商品评论，在mounthod里面配置
      this.get_comments();


//获取商品评论(在method里面配置)
      get_comments:function(){

          //发送请求
          this.axios.get('http://localhost:8000/commentlist/',{params:{gid:this.id}}).then((result)=>{

              console.log(result);

              this.commentlist = result.data;
          })
      },

      //评论
      submit:function(){

          if (this.comment==''){

              this.$Message('评论不能为空');

              return false;
          }

          this.comment = this.comment.replace(/ /g,'');

          if (this.comment.length > 100){

              this.$Message('超出了长度限制');

              return false;
          }

          //请求入库

          this.axios.post('http://localhost:8000/commentinsert/',this.qs.stringify({uid:localStorage.getItem("uid"),gid:this.id,content:this.comment})).then((result)=>{

              console.log(result);

              this.$Message(result.data.message);

              // this.get_comments();

              //添加评论数据
              this.commentlist.unshift({'uid':localStorage.getItem("uid"),'content':this.comment});
          })

      },
```


###写好代码后就可以运行啦

<video src='http://q9hs8ny3b.bkt.clouddn.com/%E5%95%86%E5%93%81%E8%AF%84%E8%AE%BA.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>
