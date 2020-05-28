---
title: 笔记(vue按钮实现切换背景颜色和字体颜色)
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



###首先要做以下的准备

![an_demo1](/hexo-personage/images/an_demo1.png)

![an_demo2](/hexo-personage/images/an_demo2.png)

![an_demo3](/hexo-personage/images/an_demo3.png)

![an_demo4](/hexo-personage/images/an_demo4.png)


###然后就是代码实现的方式

```vue

 <div>
    <!-- 切换暗黑模式开关 -->
    <h-switch v-model="k_lang" @change="change_back">白/暗</h-switch>
</div>

    // 切换模式
      k_lang:0,



            // 切换主题颜色
            change_back:function(){
              if(this.k_lang == true){
                // 获取样式表
                var styles = getComputedStyle(document.documentElement);
                // 动态更改
                document.documentElement.style.setProperty("--bg-cplor",'#292a2d');
                // 字体颜色
                document.documentElement.style.setProperty("--a-color",'white')
              }else{
                // 获取样式表
                var styles = getComputedStyle(document.documentElement);
                // 动态更改
                document.documentElement.style.setProperty("--bg-cplor",'#fff');
                // 字体颜色
                document.documentElement.style.setProperty("--a-color",'black')
              }
            },
```



###最后的实现效果就是如图所示

<video src='http://q9hs8ny3b.bkt.clouddn.com/%E5%B1%95%E7%A4%BA.mp4' type='video/mp4' controls='controls'  width='100%' height='100%'>
</video>


