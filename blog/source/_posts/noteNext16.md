---
title: 笔记(vue+django装饰器实现分页效果)
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


```vue

				<!--简单版分页容器-->
				<div>

					<Pagination layout="pager,jumper" small @change="get_goods" v-model="pagination" ></Pagination>


				</div>


      //分页器变量
      pagination:{
      		//当前页
      		page:1,
      		size:2,
      		total:4
      },


  	//获取商品（装饰器自带分页功能）
  	get_goods:function(){


      //发送请求
      this.axios.get('http://localhost:8000/goodslist/',{params:{page:this.pagination.page,size:this.pagination.size}}).then((result) =>{

        console.log(result);

        this.goodslist = result.data.data;

        this.pagination.total = result.data.total;

      });

    },


```


```python
from rest_framework.response import Response
from rest_framework.views import APIView

from myapp.models import Goods
from myapp.myser import GoodsSer

#商品列表页
class GoodsList(APIView):

    def get(self,request):

        #排序字段
        colum = request.GET.get("coloum",None)

        #排序方案
        sort_order = request.GET.get('order',None)

        #当前页
        page = request.GET.get('page',1)

        #一页显示个数
        size = request.GET.get('size',3)

        #计算从哪开始
        data_start = (int(page)-1) * int(size)

        #计算切到哪
        data_end = int(page) * int(size)

        #查询切片操作

        if colum:

            goods = Goods.objects.all().order_by(sort_order+colum)[
                data_start:data_end
            ]
        else:
            goods = Goods.objects.all()[data_start:data_end]

        #查询所有商品个数
        count = Goods.objects.count()

        goods_ser = GoodsSer(goods,many=True)

        return Response({'data':goods_ser.data,'total':count})


```
###最终实现效果


<video src='http://q9hs8ny3b.bkt.clouddn.com/%E8%A3%85%E9%A5%B0%E5%99%A8%E8%87%AA%E5%B8%A6%E5%88%86%E9%A1%B5%E6%95%88%E6%9E%9C.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>
