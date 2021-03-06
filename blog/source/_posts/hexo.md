##hexo配置并部署


---
title: hexo
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


```

首先放出我的博客地址,在fan主题的基础上进行了一些修改和替换，感谢fan主题的作者。

hexo是一个静态博客生成框架，码云是国内的一个代码托管平台（类似git）。利用码云的page服务可以很方便的部署和上线一个静态博客或者静态网站。这种page服务我最早在github上接触过，github和码云的page比较：

 - github ：github的page服务也很方便，但是部署了之后会发现访问很慢（github国外网站的原因），并且github不接收百度的seo收录。
 - 码云 ：国内的，所以访问会快一些，需要每次上传版本后手动部署（免费），如果自动部署需要一年99块钱。
对比之后选选择了码云。

```

###hexo本地搭建
```
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。 —— 中文官网

开始之前需要

nodejs开发环境（node.js,npm 等）
git（用来上传代码）
D盘下新建文件夹 blog。
建好之后，在blog文件夹中右键 git bash here。
```
####然后全局安装hexo构建命令

```
npm install hexo-cli -g
```

####然后输入
```
hexo init
```

###进行 hexo项目代码的构建
运行好之后，目录结果如下图

![blog](/hexo-personage/images/d47b-hrvcwnk0004381.png)


![blog](/hexo-personage/images/post.jpg)


![赞赏](/hexo-personage/images/赞赏.jpg)





####再次输入
```
hexo g
```

###进行静态文件编译
编译好之后会多一个public文件夹出来
图片

###再运行

```
hexo server
```

###执行完之后在浏览器中输入
```
http://localhost:4000
```

到这里本地服务器上的hexo已经全部部署完毕。

####接下来部署到码云
```
登录 码云官网,没有账号的注册一个。
点击头像边的加号，新建仓库。
```

###填写个人信息
```
在打开的页面中，输入你自己想要填写的个性信息，填写好之后点击创建。


创建好之后，点击右边的克隆/下载按钮,然后复制https地址。


然后打开blog文件夹中的_config.yml文件（项目配置文件），拉到最后找到deploy
将repo:后面的网址改成你复制的地址，保存。


接下来，运行命令 安装hexo-deployer-git模块，gitbash 运行 npm install hexo-deployer-git --save
```

###安装完毕
```
安装好之后，文件夹会多出来一个.deploy_git,然后运行命令 hexo g -d,就可以把内容推送到码云了。
推送好之后，上码云建立的仓库中可以看到编译之后的文件，然后点击服务，选择page，
开启服务，然后等他部署好之后，会出现一个网址，就是你的博客地址。

关于换了电脑如何能够快速部署
经过上面的步骤，已经实现了hexo博客的部署，有经验的工程师应该接下来都会玩了，可以不用继续看接下来的文档了。
下面我讲对加强部署做进一步说明。

你会发现你用了hexo d 命令上传到代码库的只是编译后的静态文件，如果有一天你的电脑发生了意外，文件全部取不出来了，那么重新部署本地工作环境将变得麻烦，可能需要重新下载主题插件，还有一部分插件可能你都忘了是什么了。

基于换了电脑如何快速部署这一个问题，官网建议的一个仓库里面两个分支，这也是大部分网上的解决方案，可以自行百度。

这里我提出一个新的解决方案，放弃使用hexo d 命令

原理就是把整个工作目录都上传到码云上面去，包括public文件，每次更新的博客的时候，先用hexo g 命令 进行编译 然后用git add * git commit -m XXX git push将整个目录更新到码云上面。然后在部署的时候，将根文件选择为public。

上传项目
在码云新建一个仓库，然后本地新建一个文件夹，然后clone这个仓库到本地，成功之后，复制里面的.git文件。
重新初始化一个项目，然后把这个git文件放到这一个文件夹中，然后打开git bash ，先进行hexo g，然后git add * git commit -m XXX git push，这时候项目的原始文件就已经到了你新建的仓库里面。
```
###部署完成
码云page选择
将部署目录设置为public 点击部署


