
---
title: selenium介绍
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
 - 悦读
keywords: Sakura
description: selenium
---





<h4>selenium介绍


```
#介绍
1.selenium是一个web自动化测试用的框架，程序员可以通过代码实现对浏览器的控制，比如打开网页，点击网页中的元素，实现鼠标滚动等操作.
2.它支持多款浏览器，如谷歌浏览器，火狐浏览器，当然也支持无头浏览器

#目的：
在爬取数据的过程中，经常遇到动态数据加载，一般数据加载有两种，一种通过ajax请求加载数据，另一种通过js代码加载动态数据.selenium可以模拟人操作真实的浏览器，获取加载完成的页面数据
ajax:
    url有规律且未加密，直接构建url连接请求
    uel加密无法破解规律 -->selenium
js动态数据加载 --> selenium
```



<h4>2.selenium安装

```
三要素：浏览器，驱动程序，selenium框架
	浏览器：推荐谷歌浏览器，标准稳定版本
    驱动程序：http://chromedriver.storage.googleapis.com/index.html
    pip install selenium
    
#测试：
from selenium import wedbriver
browser = wedbriver.Chrome('./chromedriver.exe') #将驱动放在脚本所在的文件夹
browser.get('https://www.baidu.com')
```

