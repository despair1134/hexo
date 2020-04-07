hexo的安装步骤


---
title: hexo搭建博客
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
description: hexo
---


##使用hexo搭建自己的静态博客

```
前提摘要
可能不需要会写多少代码，会照葫芦画瓢对一些地方进行修改就好。

安装node.js npm 了解基础知识 个人觉得不了解好像也没什么问题，跟着装就好了。然后就了解了啊。

安装git 可以不知道git是什么 hexo配合github搭建博客会用到四个常用的命令 到时候掌握了这四个命令的作用就行了

有一个github账号 和其他账号的注册没有什么区别。用github作为服务器使用。

注册地址：github地址
会markdown就好，不会也没关系，现学也可以很快掌握。

推荐markdown编辑器：typora
我的markdown学习笔记：markdown笔记
关于学习笔记，我只能保证自己看的懂，不防Google一下 （在多方影响下已经很嫌弃百度了。。。不得不说谷歌确实好用）
综上如果对敲代码一无所知，会有很多词不知道 无所谓 有个大致的印象就好

个人认为不会敲代码也是可以跟着文章搭建起来自己的博客的，如果因此想去敲代码了，嗯...我应该是想多了...

既然从零开始，我就自己新建了一个号从头开始搭 所有的操作环节都是在windows环境下进行的

只是说不用会敲代码，电脑还是要会用一些的。

最后说明 应该算是蛮基础蛮详细的 图多啰嗦慎入

```



##一、环境搭建
```
1.1 安装node.js
下载地址如下：node.js下载

下载自己所需要的版本，与安装普通软件没有任何区别。

安装node.js
检查一下是否安装成功

windows系统下快捷键win+r 输入cmd

打开终端
在终端输入 npm -v 显示版本号则为安装成功 版本号不用和我的一样

检查node版本
1.2 安装git
下载地址及安装方法如下：git安装 安装地址和方法这里都有说了

若是安装成功 鼠标右键会出现这俩图表

git安装
这两个是必备的工具 一定要安装上才行
```

###二、创建库
```
2.1注册gitbub账号
本来不想从注册开始写的...然后想了想不能做一个标题党，所以真的从零开始哦。

① 输入地址 点击注册

github注册地址
② 填写表单

填写表单
③ 继续 账号也就有了

注册
2.2 建立仓库
① 新建项目

新建项目
② 上邮箱里验证一下

验证
③新建项目

新建一个名为你的用户名.github.io的仓库，例如，你的github用户名为user，就新建一个名为user.github.io的仓库（强制要求哦），将来你的网站访问地址就为：http://user.github.io 并且一个github账户最多只能创建一个这样可以直接使用域名访问的仓库。

项目命名
2.3 绑定域名
绑也行，不绑也行。绑的话首先你要有个域名 我是在阿里云上买的。我的博客域名：[www.theonefish.com](www.theonefish.com,'my blog')

下面说一下怎么绑，2.3这一步是完全可以跳过的

①首先购买域名。

②解析域名。

在你的域名服务平台的控制台进行解析，设置两条记录，记录的类型都选择 CNAME，记录值都选择user.github.io，主机记录一条为空，另一条填写 www。

关于域名的购买和域名的解析不做更详细的说明。

③设置pages

在github上打开你的工程，点击 Settings 。

settings
然后找到 GitHub Pages 下的 Custom domain ，填写你的 www 域名，点击 save ，这时域名就可以访问到你的网站了。

pages
④修复本地工程

GitHub工程里会出现一个CNAME文件里面写着你域名，见这个文件放到你的本地工程的所用主题下的source文件夹下，不然下次发布时会清空这个文件。(这个后边会再强调 先有个印象，现在不用操作)

2.4 配置SSH KEY
我们到时候会把本地的代码提交到github上，这一步就是配置权限，解决本地和服务器的链接问题

① 鼠标右键git bash here

配置步骤1
②打开git bash，就是一个linux命令窗口

配置步骤2
③创建SSH keys

在bash中输入

ssh-keygen -t rsa -C "shaoshanhuan@163.com"

然后无脑回车~

配置成功
④复制keys

进入生成的ssh目录 : C:\users(中文是用户)\你电脑的用户名 .ssh 中, 使用记事本打开 id_rsa.pub 文件, 将该文件中的内容复制

复制keys
⑤复制id_rsa.pub里面的内容

id_rsa.pub
⑥将复制的内容粘进github

配置1
配置2
配置3
⑦测试是否成功

ssh -T git@github.com 
如果提示Are you sure y