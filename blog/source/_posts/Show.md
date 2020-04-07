---
title: python使用教程
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
description: python
---

---
    01-python要点
        1 python语言
            1.1 python语言的基本概念
                1.2 python的特色
                    1.3 python的发展及应用
        2 python搭建环境
            2.1 python的解释器
                2.2 搭建python环境
                    2.3 python的交互模式
                        2.4 pip的工具使用
                            2.5 Python的第一个程序
        3.PyCharm的安装和配置
            3.1 3.1PyCharm的安装
                3.2 pycharm简单配置
---

## 1 python语言的简单介绍
```
1.1 python语言的基本概念
Python 是一种极少数能兼具 简单 与 功能强大 的编程语言。你将惊异于发现你正在使用的这门编程语言是如此简单，它专注于如何解决问题，而非拘泥于语法与结构

官方对 Python 的介绍如下：
– Python 是一款易于学习且功能强大的编程语言。 它具有高效率的数据结构，能够简单又有效地实现面向对象编程。Python 简洁的语法与动态输入之特性，加之其解释性语言的本质，使得它成为一种在多种领域与绝大多数平台都能进行脚本编写与应用快速开发工作的理想语言

Python 的创造者吉多·范罗苏姆（Guido van Rossum）采用 BBC 电视节目《蒙提·派森的飞行马戏团（Monty Python’s Flying Circus，一译巨蟒剧团）》的名字来为这门编程语言命名


1.2 python的特色
简单
易于学习
自由且开放
跨平台
可嵌入性
丰富的库
1.3 python的发展及应用
```


###python的应用

 - 1.常规软件开发
 - 2.科学计算
 - 3.自动化运维
 - 4.自动化测试
 - 5.WEB开发
 - 6.网络爬虫
 - 7.数据分析
 - 8.人工智能
 - Python之禅


###python的简易操作

```
(输入import this )
译文：
美胜于丑陋（Python 以编写优美的代码为目标）
明了胜于晦涩（优美的代码应当是明了的，命名规范，风格相似）
简洁胜于复杂（优美的代码应当是简洁的，不要有复杂的内部实现）
复杂胜于凌乱（如果复杂不可避免，那代码间也不能有难懂的关系，要保持接口简洁）
扁平胜于嵌套（优美的代码应当是扁平的，不能有太多的嵌套）
间隔胜于紧凑（优美的代码有适当的间隔，不要奢望一行代码解决问题）
可读性很重要（优美的代码是可读的）
即便假借特例的实用性之名，也不可违背这些规则（这些规则至高无上）
不要包容所有错误，除非你确定需要这样做（精准地捕获异常，不写 except:pass 风格的代码）
当存在多种可能，不要尝试去猜测而是尽量找一种，最好是唯一一种明显的解决方案（如果不确定，就用穷举法）
虽然这并不容易，因为你不是 Python 之父（这里的 Dutch 是指 Guido ）
做也许好过不做，但不假思索就动手还不如不做（动手之前要细思量）
如果你无法向人描述你的方案，那肯定不是一个好方案；反之亦然（方案测评标准）
命名空间是一种绝妙的理念，我们应当多加利用（倡导与号召）
```



###2 python搭建环境
```
2.1 python的解释器
环境搭建就是安装Python的解释器
Python的解释器分类：
① CPython（官方我们用的就是这个版本） 用c语言编写的Python解释器
② PyPy 用Python语言编写的Python解释器
③ JPython 用Java编写的Python解释器
2.2 搭建python环境
1.下载视频
2.官网链接

下载安装包
1.点击Downloads,选择自己的电脑版本


选择符合自己电脑位数的版本

在这里我推荐最好用3.7或3.6版本，比较稳定，尽量不要再下载2版本了，因为2版本快要停更了
```


###安装
```
1.点击自定义安装
2.并打勾（添加到环境变量
3.根据自己的条件打勾（个人建议全勾）
4.勾选第四个并选择自己所执导的路径
5.打开Dos命令输入 python -V 验证是否安装成功
2.3 python的交互模式

• win键 + R --> CMD --> 回车 --> 输入Python
• 命令行结构：
1 .Python 3. 6. 5 … —> 版本
2 .Type “help”,“copyright”…—> 版权声明
3 . >>> —> 命令提示符 (在后面可以直接输入指令)

2.4 pip的工具使用
pip介绍
我们都知道python有很多的第三方库或者说是模块。这些库针对不同的应用，发挥不同的作用。我们在实际的项目中肯定会用到这些模块。那如何将这些模块导入到自己的项目中呢？
Python官方的PyPi仓库为我们提供了一个统一的代码托管仓库，所有的第三方库，甚至你自己写的开源模块，都可以发布到这里，让全世界的人分享下载 。
python有两个著名的包管理工具easy_install和pip。在python 2中easy_install是默认安装的，而pip需要我们手动安装。随着Python版本的提高，easy_install已经逐渐被淘汰，但是一些比较老的第三方库，在现在仍然只能通过easy_install进行安装。目前，pip已经成为主流的安装工具，自Python 2 >=2.7.9或者Python 3.4以后默认都安装有pip

pip使用
在命令行下，输入pip，回车可以看到帮助说明：

查看pip版本

在命令提示符中输入pip -V 或pip --version

pip -V
pip --version
1
2

1.普通安装
在命令提示符中输入pip install xxx 在这里xxx表示你所要安装的模块

pip install requests  
1
2.指定版本安装

pip install robotframework==2.8.7
1
3.写在已安装的库
pip uninstall requests

pip install SomePackage             
pip install SomePackage==1.0.5       # 指定版本
pip install 'SomePackage>=1.0.6'     # 最小版本
1
2
3
升级指定的包，通过使用==, >=, <=, >, < 来指定一个版本号

4.列出已安装的库

pip list
1
5.显示所安装包的信息

pip show package
1
6.将已经安装的库列表保存到文本文件中

pip freeze > D:\桌面\install.txt
1

7.使用wheel文件安装

除了使用上面的方式联网进行安装外，还可以将安装包也就是wheel格式的文件，下载到本地，然后使用pip进行安装。比如我在PYPI上提前下载的pillow库的wheel文件，后缀名为whl

地址:https://www.lfd.uci.edu/~gohlke/pythonlibs/

可以使用pip install pillow-4.2xxxxxxx.whl的方式离线进行安装

第一步 安装 wheel

第二步 找到下载的whl文件的目录进行安装(以桌面为例)

第三步 执行命令安装
2.5 Python的第一个程序
可以在交互模式实现
可以用Python自带的idle
可以用高级开发工具如 : PyCharm
3.PyCharm的安装和配置
3.1 3.1PyCharm的安装
官方网址:pycharm
```






###3.2 pycharm简单配置
```
• 1、主题修改 File–settings–apperance–theme

• 2、代码字体修改 File–settings–Editor-Font

• 3、关闭更新 File–settings—apperance—System Settings —Updates — Automatically check updates for 取消打钩

• 4、快捷键修改 File–settings—apperance-- Keymap 选择自己习惯的快捷键方式

• 5、自动导包 File–settings—apperance–General —Auto Import 打钩

• 6、进制打开上次的工程 File–settings—apperance—System Settings —Reopen last project startup

• 7、修改新建文件文件头 File–settings–Editor—Code Style — File and Code Templates — Python Script
```

