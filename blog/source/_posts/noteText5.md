
---
title: 笔记(【Python+selenium】带图片验证码的登录自动化实战)
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


 - 近期在跟进新项目的时候，整体的业务线非常之长，会一直重复登录退出不同账号的这个流程，所以想从登录开始实现部分的自动化。因为是B/S的架构，所以采用的是selenium的框架来实现。大致实现步骤如下：
　　 - 1.环境准备
　　 - 2.验证码爬取
　　 - 3.识别方案选择
　　 - 4.图像处理和识别
　　 - 5.自动化实现
<h2>一、环境准备
　　 - 系统：Windows
　　 - 软件：Pycharm
　　 - 语言：Python 3.7
　  - 浏览器：Chrome 70.0.35
　　 - 依赖库：selenium 3.141、xlrd 1.1、aip 1.0.0.5、pytesser、pytesseract 0.2.5、opencv-python 3.4.3、urllib3 1.24.1、Pillow-PIL 0.1

<h3>驱动安装与配置环境：
    ① 下载chromedriver:http://chromedriver.storage.googleapis.com/index.html（需代理）、http://npm.taobao.org/mirrors/chromedriver/（无需代理）
    ②具体浏览器与驱动版本映射表可参考 https://blog.csdn.net/huilan_same/article/details/51896672 ,最新chrome 74版本---ChromeDriver v74.0.3729.6
    ③解压后放置在/usr/local/bin/目录下
    ④加入环境变量：export PATH=$PATH:/usr/local/bin/ChromeDriver
<h3>二、验证码爬取
    对于验证码而言，目前各式网站出现的验证码类型基本有：图形验证码（数字、计算题、中文、英文、问答题）、滑块验证码、语音验证码、图片验证码（正倒序、同类型)。自身项目的验证码为数字+英文图形验证码，针对这一块的内容，首先我们先来爬取一些验证码到指定文件夹中，来着重分析一下特点。代码如下：

```bash
#-*- coding:utf-8 -*-
from selenium import webdriver
import time
import urllib
import os
import sys


req_url = "https://项目网址/#/"


def download_code(num):
   for i in range(int(num)):
       browser.refresh()
       time.sleep(3)
       # 寻找登录按钮，查找登录classname
       browser.find_elements_by_css_selector("[class='ant-btn logining-btn ant-btn-primary ant-btn-lg ant-btn-background-ghost']")[0].click()
       time.sleep(3)
       #获取验证码url链接
       src=browser.find_elements_by_css_selector("[class='picturecode-img']")[0].get_attribute("src")
       time.sleep(1)
  
       local = '/Users/funny/PycharmProjects/auto_cloud/code_pic/' + str(i) + '.png'
       print local
       urllib.urlretrieve(src,local)
       time.sleep(1)

if __name__=="__main__":
   browser = webdriver.Chrome()
   browser.get(req_url)
   download_code(sys.argv[1])
   browser.close()
   
```
 
大致讲解一下上面出现的一些函数用法和实现过程中存在的问题。
　　1.使用classname定位，运行时报错
　　A：一般来说，使用classname来定位还是比较精准的，但是此项目的classname包含了多个tag，如上述的登录按钮class='ant-btn logining-btn ant-btn-primary ant-btn-lg ant-btn-background-ghost'，这时候使用 find_elements_by_class_name方法定位，会无法定位并报错。所以需要使用find_elements_by_css_selector，大家可以根据各自项目来选择方法。
　　2.urllib.urlretrieve(src,local)
　　urllib模块提供的urlretrieve()函数，urlretrieve()方法直接将远程数据下载到本地，传入下载的链接。
　　3.命令行获取参数
　　为了指定我们想要下载的验证码数量，要在源程序里面修改吗？不用。sys.argv[]是一个从程序外部获取参数的桥梁，所获得的是一个列表（list)，文中的sys.argv[1]则是代表获取列表中的下标为1的内容，在终端我们运行的方法是：python catch_code.py 10 ，这样sys.argv[1]取到的的值则为10,num的值亦为10，循环10次下载验证码。

<h3>三、识别方案选择
上节中爬取下来了100张验证码，如下图：
![deco](/hexo-personage/images/deco.png)


基本特性是：横向排列、数字与英文字母组合、字母间粘连占比约30%、背景干扰较少。阅读已有的一些ocr识别技术，基本有以下三个方向：
　　　　① pytesser
　　　　② pytesseract
　　　　③ 百度文字识别 AipOcr
　　为了对比这三者识别技术的识别率，对应实现来展示效果，所以样本选择为0.png、4.png、11.png（字母粘连、纯字母、字母+数字）
　　pytesser：谷歌OCR开源项目的一个模块，在python中导入这个模块即可将图片中的文字转换成文本。pytesser下载链接：http://code.google.com/p/pytesser/ ，实现代码如下：

```bash
#-*- coding:utf-8 -*-
from PIL import Image
import pytesser.pytesser as pytesser

image = Image.open('code_pic/test_pic/0.png')
print pytesser.image_file_to_string('code_pic/test_pic/0.png')
print pytesser.image_to_string(image)
image_file_to_string()函数可以实现简单的英文字母识别，如果图像是不相容的，会先转换成兼容的格式，然后再提取图片中的文本信息。
　　
```
image_to_string()函数亦可实现英文字母识别，读取图片时，将内存中的图像文件保存为bmp，再使用tesseract处理。
　　执行结果如下：
![demo1](/hexo-personage/images/demo1.png)


顺序识别0,4,11图片后均无法识别结果，识别概率为0%
　　pytesseract:Google的Tesseract-OCR引擎包装器

```bash
print pytesseract.image_to_string(Image.open('code_pic/test_pic/11.png'),lang="eng")
```

顺序识别0,4,11图片后均无法识别结果，识别概率为0%
　　AipOcr：一款百度提供的OCR识别服务，支持多种图片格式，接口免费调用50000次/日，具体请参考官方文档：https://ai.baidu.com/docs#/OCR-API/top ，在实现之前，我们需要创建一款产品，来获得AppID、API Key、Secret Key的值。如下图：
![demo2](/hexo-personage/images/demo2.png)

获取到以上三个参数后，继续上代码：

```bash
from aip import AipOcr

#  你的 APPID AK SK
APP_ID = '1*****'
API_KEY = 'sHzo*******'
SECRET_KEY = 'V******'

client = AipOcr(APP_ID, API_KEY, SECRET_KEY)
# 读取图片
def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()

image = get_file_content('/Users/funny/PycharmProjects/auto_cloud/code_pic/test_pic/11.png')
#  调用通用文字识别, 图片参数为本地图片
result = client.general(image)

# 定义参数变量
options = {
    # 定义图像方向
        'detect_direction' : 'true',
    # 识别语言类型，默认为'CHN_ENG'中英文混合
        'language_type' : 'CHN_ENG',


}

# 调用通用文字识别接口
result = client.general(image,options)
print(result)
for word in result['words_result']:
    print(word['words'])
    
```    
    
顺序识别0,4,11图片后,图片11识别出了一半,提取到了"2F"，概率为16%
<h3>四、图像处理和识别
在上节看来，未经过处理的图片进行识别，识别概率都非常之低。所以我们换一个角度来思考，通过对图片进行一些处理，使得特征更加明显，再通过上述的三种识别库来识别，提高识别的概率。步骤大致如下：1）灰度二值化 2）线降噪 3）开运算
　　1）灰度二值化

```bash
im = cv2.imread('/Users/funny/PycharmProjects/auto_cloud/code_pic/0.png')
im = cv2.cvtColor(im,cv2.COLOR_BGR2GRAY) 
# 二值化
th1 = cv2.adaptiveThreshold(im, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C, cv2.THRESH_BINARY, 21, 1)
edges = cv2.Canny(th1, 30, 70)
cv2.imshow('二值化',th1)
cv2.waitKey(0)
cv2.destroyAllWindows()

```
处理的图像如下：
![demo3](/hexo-personage/images/demo3.png)

2）线降噪

```bash
#二值化图片，并且线降噪
img = cv2.imread('/Users/funny/PycharmProjects/auto_cloud/code_pic/11.png')
img = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY) #灰值化
h, w = img.shape[:2]
# opencv矩阵点是反的
# img[1,2] 1:图片的高度，2：图片的宽度
for y in range(1, w - 1):
    for x in range(1, h - 1):
        count = 0
        if img[x, y - 1] > 245:
            count = count + 1
        if img[x, y + 1] > 245:
            count = count + 1
        if img[x - 1, y] > 245:
            count = count + 1
        if img[x + 1, y] > 245:
            count = count + 1
        if count > 2:
            img[x, y] = 255


cv2.imshow('线降噪',img)
cv2.waitKey(0)
cv2.destroyAllWindows()

```
处理的图像如下：
![demo4](/hexo-personage/images/demo4.png)

3)闭运算
```bash
img = cv2.imread('/Users/funny/PycharmProjects/auto_cloud/code_pic/11.png')
img = cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (2, 3))  # 定义结构元素
opening = cv2.morphologyEx(img, cv2.MORPH_OPEN, kernel)  # 开运算
closing = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)
cv2.imshow('闭运算',closing)

cv2.waitKey(0)
cv2.destroyAllWindows()

```
处理的图像如下：

![demo5](/hexo-personage/images/demo5.png)


图像处理到现在基本上我们已经将已有的背景干扰及色彩去除完毕，接下来我们针对这些处理的图像进行三种识别方案的识别，识别结果如下表：

![demo6](/hexo-personage/images/demo6.png)

我们来分析一下这个表，在最开始的二值化，AipOcr至少识别出来了一些内容。纵观三种图像处理后的识别效果，明显闭运算已经能识别出大致的内容了，图片4.png三种识别方式都是可以识别出来，对于0.png这种粘连字母，识别效果基本为0%，而11.png“j”的底部表现不出来，所以识别不出来，但后面的内容亦识别成功。所以我们可以总结三点：①识别方式精准度 ：AipOcr>pytesser>pytesseract。 ②处理后效果：闭运算>线降噪>二值化。③粘连性、带噪点图片识别效果非常差（当前准确值是基于我选取的样本集）。

<h3>五、自动化实现

从上节的处理和识别中的总结内容中，本项目我们选择将AipOcr作为识别，若识别结果不正确（如粘连、噪点过多、部分裁剪图片），将获取新的验证码，以此类推。将上述部分代码封装，方便调用，最终完整代码如下：

```bash
#-*- coding:utf-8 -*-
from selenium import webdriver
from time import sleep
import xlrd
import os
import time
import urllib
import cv2
from aip import AipOcr
#define
req_url = "网址"
local = '/Users/funny/PycharmProjects/auto_cloud/code_pic/code.png'
APP_ID = '1****2'
API_KEY = 's*****'
SECRET_KEY = 'V******Hw'
xlsname="user_tab.xlsx"

#excel读取
def Load_excel():
    excel = xlrd.open_workbook(xlsname)
    shxrange = range(excel.nsheets)
    try:
        sh = excel.sheet_by_name("Sheet1")
    except:
        print "no sheet in %s named Sheet1" % xlsname
    nrows = sh.nrows
    ncols = sh.ncols
    #print "nrows %d, ncols %d" % (nrows, ncols)
    # 获取第一行第一列数据
    cell_value = sh.cell_value(1, 1)
    # print cell_value
    row_list = []
    # 获取各行数据
    for i in range(1, nrows):
        row_data = sh.row_values(i)
        row_list.append(row_data)
    return row_list

def change_catch():
    img = cv2.imread('/Users/funny/PycharmProjects/auto_cloud/code_pic/code.png')
    img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (2, 3))  # 定义结构元素
    closing = cv2.morphologyEx(img, cv2.MORPH_CLOSE, kernel)
    cv2.imwrite('/Users/funny/PycharmProjects/auto_cloud/code_pic/code-close.png',closing)

def code_detect():
    client = AipOcr(APP_ID, API_KEY, SECRET_KEY)
    f=open('/Users/funny/PycharmProjects/auto_cloud/code_pic/code-close.png','rb')
    image =f.read()
    #  调用通用文字识别, 图片参数为本地图片
    result = client.general(image)
    # 定义参数变量
    options = {
    # 定义图像方向
        'detect_direction': 'true',
        # 识别语言类型，默认为'CHN_ENG'中英文混合
        'language_type': 'CHN_ENG',
    }
# 调用通用文字识别接口
    result = client.general(image, options)
    print result
    print str(result['words_result'][0]['words'])
    return str(result['words_result'][0]['words'])






if __name__ == '__main__':

    flag=False
    row_list=Load_excel()
    print row_list
    browser = webdriver.Chrome()
    browser.get(req_url)
    time.sleep(4)
    #寻找登录按钮，查找登录classname
    browser.find_elements_by_css_selector("[class='ant-btn logining-btn ant-btn-primary ant-btn-lg ant-btn-background-ghost']")[0].click()
    time.sleep(2)
    #获取验证码url
    src = browser.find_elements_by_css_selector("[class='picturecode-img']")[0].get_attribute("src")
    urllib.urlretrieve(src, local)
    print "下载验证码中。。。"
    change_catch()
    word=code_detect()
    print word
    time.sleep(1)
    browser.find_element_by_id("loginName").send_keys(row_list[0][1])
    browser.find_element_by_id("password").send_keys(row_list[0][2])
    browser.find_element_by_id("imgValidCode").send_keys(word)
    browser.find_elements_by_css_selector("[class='ant-btn ant-btn-primary ant-btn-lg ant-btn-block']")[0].click()
    time.sleep(1)

    while browser.current_url=="网址":
        time.sleep(2)
        src = browser.find_elements_by_css_selector("[class='picturecode-img']")[0].get_attribute("src")
        urllib.urlretrieve(src, local)
        print "下载验证码中。。。"
        change_catch()
        word = code_detect()
        time.sleep(2)
        browser.find_element_by_id("imgValidCode").send_keys(word)
        browser.find_elements_by_css_selector("[class='ant-btn ant-btn-primary ant-btn-lg ant-btn-block']")[0].click()

    print "登录成功"
```

对于粘连性及部分被切割的验证码，还需要再研究一番~
另，因为验证码识别率还不能达到100%，且后期可能因为版本迭代的原因，更换不同方式的验证码类型，所以这里只是提供一个图像预处理思路给到大家，实现登录自动化还有其他方式，如白名单控制、关闭验证码校验等。
