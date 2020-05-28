---
title: 笔记(vue+django实现检索功能)
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

# 导包
from django.db.models import Q, F
from rest_framework.response import Response
from rest_framework.views import APIView

from myapp.models import Goods
from myapp.myser import GoodsSer


#商品列表页
class GoodsList(APIView):

    def get(self,request):

        #检索字段
        text = request.GET.get('text',None)

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
        #判断是否进行模糊查询
        if text:
            goods = Goods.objects.filter(Q(name__contains=text) | Q(desc__contains=text))[data_start:data_end]

            count = Goods.objects.filter(Q(name__contains=text) | Q(desc__contains=text)).count()
        else:

            #查询所有商品个数
            count = Goods.objects.count()

        goods_ser = GoodsSer(goods,many=True)

        return Response({'data':goods_ser.data,'total':count})


```



```vue


<template>
  <div>

    <myheader></myheader>



		<section class="products text-center">
			<div class="container">
				<h3 class="mb-4">商品检索</h3>
				<div class="row">

					<div v-for="item in goodslist" class="col-sm-6 col-md-3 col-product">
						<figure>
							<img :src="'http://localhost:8000/static/upload/'+item.img" class="rounded-corners img-fluid" 	width="240" height="240">
							<figcaption>
								<div class="thumb-overlay"><a href="item.html" title="More Info">
									<i class="fas fa-search-plus"></i>
								</a></div>
							</figcaption>
						</figure>
						<h4><a :href="'http://localhost:8080/item?id='+item.id">

              <div v-html="$options.filters.myfilter(item.name)"></div>

            </a></h4>
						<p><span class="emphasis">${{ item.price }}</span></p>
					</div>

				</div>

				<!--简单版分页容器-->
				<div>

					<Pagination layout="pager,jumper" small @change="get_goods" v-model="pagination" ></Pagination>


				</div>



				</div>


		</section>

		<div class="divider"></div>


		<footer class="footer">

		<div class="container">
			@v3u.cn
		</div>


	</footer>
    </div>


</template>



<script>

//导入组件
import myheader from './myheader.vue'

export default {
  data () {
    return {
      msg: "这是一个变量",
      //分页器变量
      pagination:{
      		//当前页
      		page:1,
      		size:2,
      		total:4
      },
        //商品列表
        goodslist:[],
        //搜索关键字
        text:'',
    }
  },
  //注册组件标签
  components:{

  	'myheader':myheader

  },
  mounted:function(){

      //传递公共变量
      window.that = this;

      //接收参数
      var text = this.$route.query.text;

      console.log(text);

      //判断
      if(text.indexOf(' ')){
          text = text.split(" ");

          //格式转换
          text = JSON.stringify(text);
      }

      this.text = text;

      console.log(text);


  	this.get_goods();


},

    //自定义过滤器
    filters:{

      myfilter:function (value) {
          console.log(window.that.text);

          value = value.replace(new RegExp(window.that.text,'g'),'<span class="highlight">'+window.that.text+'</span>');

          return value;

      }
    },
  methods:{

  	//获取商品（装饰器自带分页功能）
  	get_goods:function(){


      //发送请求
      this.axios.get('http://localhost:8000/goodslist/',{params:{page:this.pagination.page,size:this.pagination.size,text:this.text}}).then((result) =>{

        console.log(result.text);

        // for (let i=0;i<result.data.data.length;i++){
        //
        //     result.data.data[i]['name'] = result.data.data[i]['name'].replace(new RegExp(this.text,'g'),'<span class="highlight">'+this.text+'</span>');
        // }

        this.goodslist = result.data.data;

        this.pagination.total = result.data.total;

      });

    },

    }

}


</script>

<style>

  .highlight{
    color: pink;
  }


</style>

```



###最终实现效果如下图所示


<video src='http://q9hs8ny3b.bkt.clouddn.com/%E5%95%86%E5%93%81%E6%A3%80%E7%B4%A2.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>

