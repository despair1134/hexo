

---
title: request高级用法
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
description: request
---


###requests高级用法


```​	
    文件上功能

​	cookie处理

​	会话维持与模拟登陆

​	SSL证书验证

​	代理设置

​	超时设置

​	构建Reques对象
```

###urllib简单介绍


- 	正则(复习)

- ​	正则基础语法

###requests高级用法




![1571800994376](/hexo-personage/images/1571800994376.png)





###Cookie用法


![1571801236089](/hexo-personage/images/1571801236089.png)




###SSL证书验证SSL证书验证


```
SSL证书验证

requests提供了证书验证的功能，当发送HTTP请求时，模块会检查SSL证书，但检查的行为可以用verify参数来控制。

​	verity = False  不检查SSL证书

​	verify = Ture  检查SSL证书



异常

如果使用requests模块的SSL验证，验证不通过会抛出异常，此时可以将verify参数设置为False
```



###代理设置


```
代理：代理即代理ip

代理ip是在请求的过程中使用非本机ip进行请求，避免大数据量频繁请求的过程中出现ip封禁，限制数据的爬取
```



###代理ip分类


```​	
    1.透明代理ip：请求时，服务器知道请求的真实ip，知道请求使用了代理

​	2.匿名代理ip：请求时，服务器知道请求使用了代理，但不知道请求的真实ip

​	    3.高匿代理ip：请求时，服务器不知道请求使用了代理，也不知道请求的真实ip
```
###requests模块使用代理ip



![1571802943736](/hexo-personage/images/1571802943736.png)




###RE模块

```
re.findall('正则表达式',‘带匹配字符串’)：返回所有满足匹配条件的结果，以列表形式返回

re.search('正则表达式','带匹配字符串')：匹配到第一个就返回一个对象，该对象使用group()进行取值，如果未匹配到则返回None

re.match('正则表达式','待匹配字符串')：只从字符串开始进行匹配，如果匹配成功返回一个对象，同样使用group()进行取值，匹配不成功返回None

re.compile('正则表达式')：将正则表达式编译未对象，在需要按该正则匹配是可以直接使用该对象调用以上方法即可



python语言：

先解释在执行：源代码->简单的翻译->字节码->二进制语言->识别的语言.pyc文件：执行过的文价，生成一个.pyc文件，再执行时对比

C：编译型语言：

源代码->编译->二进制文件->识别的语言
```


###urllib简单介绍


```
1.urllib模块是python的一个请求模块

2.python2中是urllib和urllib2相结合实现请求的发送，python3转统一为urllib库

3.urllib是python内置的请求库，其包含4个模块

​	1）.requests模块：模块发送请求

​	2）.error模块：异常处理模块

​	3）.parse模块：工具模块，提供关于URL的处理非法，如拆分，解析，合并等

​	4）.robotparser模块：识别robots协议
```


###数据解析

```
数据解析：

​	1.正则提取：根据正则表达式提取信息

​	2.xpath：定位节点，利用方法或属性来提取标签的文本内容，标签的属性值

​	3.BS4：定位，提取
```


###正则


![1571829100331](/hexo-personage/images/1571829100331.png)

![1571829134060](/hexo-personage/images/1571829134060.png)

![1571829151018](/hexo-personage/images/1571829151018.png)



![1571829186186](/hexo-personage/images/1571829186186.png)



![1571829212820](/hexo-personage/images/1571829212820.png)



![1571829231503](/hexo-personage/images/1571829231503.png)
