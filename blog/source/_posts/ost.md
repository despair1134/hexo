
---
title: OSI七层模型
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
description: OSI
---




##OSI七层模型


###常用命令

![890098](/hexo-personage/images/890098.png)



###安装命令
pip install viratualenvwrapper-win	(安装命令)


###常用命令：
```
mkvirtualenv 名称  创建虚拟环境把并自动切换到该环境下

workon 名称  切换到某虚拟环境

pip list 

rmvirtualenv 名称  删除虚拟环境

deactivate 退出虚拟环境

lsvirtualenv 列出所有常见的虚拟环境

mkvirtualenv -- python==C:\...\python.exe 名称   指定python解释器创建虚拟环境
```



###OIS七层模型


####应表会传网数物


![b21bb051f8198618b8f0ae2b40ed2e738ad4e6ee](/hexo-personage/images/b21bb051f8198618b8f0ae2b40ed2e738ad4e6ee.png)



<h2>TCP/IP


```    
    应用层：HTTP/HTTPS协议
    
    传输层：UDP(用户数据报协议)，TCP(传输控制协议)
    
    网络层:IP(英特网协议)，ICMP(控制报文协议),ARP(地址解析协议)，RARP(反向地址转换协议)
    
    数据链路层：ARP协议，逻辑控制自层(LLC),媒体访问控制自层(MAC)
    
    物理层：以太网协议
    
    
```


<h2>HTTP与HTTPS协议的区别(背会):

```
1).https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用

2).http是超文本传输协议 ，信息是明文传输，https则是具有安全性的ssl加密传输协议

3).http和https使用的完全不同的连接方式，用的端口也不一样，前者是80，后者是443

4).http的连接很简单，是无状态的：HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证网络协议，比http协议安全。

```



<h2>服务器常见端口

```
1)、ftp：File Transfer Protocol的缩写，即文件传输协议.端口：21

2)、ssh:Secure Shell的缩写，用于远程登录回话.端口：22

3）、MySql:关系型数据库,端口：3306

4）、MongoDB:非关系型数据库，端口：27017

5)、Redis:非关系型数据库，端口：6379
```




<h2>TCP/UDP
```

1.TCP协议：是一种面向连接的，可靠的，基于字节流的传输层通信协议

​	1).有序性：数据包编号，判断数据包的正确次序

​	2).正确性：使用checksum函数，检查数据包是否损坏，发送接受是都会计算校验和

​	3).可靠性：发送端有超时重发，并由确认机制识别错误和数据的丢失

​	4).可控性：滑动窗口协议与拥塞控制算法控制数据包的发送速度

2.UDP协议：用户数据报协议，面向无连接的传输协议，传输不可靠

​	1).无连接，数据可能丢失或损坏

​	2).报文小，传输速度快

​	3).吞吐量大的网络传输，可以在一定程度上承受数据丢失

```

<h2>请求方法:常见有8种

```
GET:请求页面，并返回页面内容 (重点)

POST:用于提交表单或上传文件，数据包在请求体种 (重点)

PUT:从客户端向服务器传送的数据取代指定文档中的内容

DELETE:请求服务器删除指定的页面

HEAD:类似于GET请求，只不过返回的响应没有具体的内容，用于获取报头

CONNECT:把服务器当做跳板，让服务器替代客户端访问其他网友

OPTIONS:允许客户端查看服务器的性能

TRACE:回显服务器收到的请求，主要用于测试或诊断
```



<h2>请求头

```
Cooike：也重用复数形式Cookie，这是网站为了辨别用户进行会话跟踪而存储在用户本地的数据

Accept:请求报头域，用于指定客户端可接受那些类型信息



Referer:此内容用来标识这个请求是从哪个页面发来的，服务器可以拿到这一信息并做相应的处理，如作来源统计、防盗链处理等

User-Agent：简称UA，它是一个特殊字符串头，可以是服务器识别客户使用的操作

```