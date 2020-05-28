---
title: 笔记(vue+django实现分页效果)
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
from rest_framework.response import Response
from rest_framework.views import APIView

from myapp.models import Goods
from myapp.myser import GoodsSer

#商品列表页
class GoodsList(APIView):

    def get(self,request):

        #当前页
        page = request.GET.get('page',1)

        #一页显示个数
        size = request.GET.get('size',3)

        #计算从哪开始
        data_start = (int(page)-1) * int(size)

        #计算切到哪
        data_end = int(page) * int(size)

        #查询切片操作
        goods = Goods.objects.all()[data_start:data_end]

        #查询所有商品个数
        count = Goods.objects.count()

        goods_ser = GoodsSer(goods,many=True)

        return Response({'data':goods_ser.data,'total':count})

```


```vue

<!--自主分页-->
        <br /><br />
					<div>

						<a @click="get_goods(lastpage)" v-show="lastpage" >上一页</a>

						<ul>

							<li v-for="index in all">

								<a @click="get_goods(index)"  >{{ index }}</a>

							</li>

						</ul>

						<a @click="get_goods(nextpage)" v-show="nextpage" >下一页</a>



				</div>


            //自主分页
            total:0,//商品总数
            cur:1,//当前页
            all:0,//总页数
            lastpage:0,//上一页
            nextpage:0,//下一页
            size:3,//每页显示几条数据
            datalist:[],


        //获取商品
        get_goods: function (mypage) {

            //发送请求
            this.axios.get('http://localhost:8000/goodslist/',{params:{page:mypage,size:this.size}}).then((result) => {
                console.log(result);

                this.goodslist = result.data.data;
                this.datalist = result.data.goodslist;
              //判断上一页
              if(mypage==1){
              	this.lastpage = 0;
              }else{
              	this.lastpage = mypage - 1;
              }
              //计算总页数
              this.all = Math.ceil(result.data.total / this.size);
              //判断下一页
              if(mypage==this.all){
              	this.nextpage = 0;
              }else{
              	this.nextpage = mypage + 1;
              }
            });
        },


```


###效果展示


<video src='http://q9hs8ny3b.bkt.clouddn.com/%E5%88%86%E9%A1%B5.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>

