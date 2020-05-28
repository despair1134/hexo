
---
title: scrapy中间件
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
description: scrapy
---



<h4>scrapy中间件


```
#中间件分类：
	- 下载中间件：DownloadMiddeware
    - 爬虫中间件：SpiderMiddleware
#中间件的作用：
	- 下载中间件：拦截请求与响应，篡改请求与响应
     - 爬虫中间件：拦截请求与响应，拦截管道item，篡改请求与响应，处理item

#下载中间件的主要方法：
process_request：拦截的事非异常的请求
process_response：拦截的事所有的响应
process_exception：拦截的是异常的请求
```



<h4>基于crawlSpider的全站数据爬取


```
#项目的创建
scrapy startproject projectname
scrapy genspider -t spidername baidu.com
```

```
#crawlspider全站数据爬取：(scrapy.spider -- crawlspider)
-crawspider是一个爬虫类，是scrapy.spider的子类，功能比spider更强大
-crawlspider的机制：
	-连接提取器：可以根据指定的规则连接进行提取
    -规则解析器：根据指定的规则对响应数据进行解析
```

