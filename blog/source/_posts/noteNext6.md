
---
title: 笔记(python中使用selenium自动登录功能)
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

![[VOPJAZOOS`$9IT@D(12](/hexo-personage/images/VOPJAZOOS`$9IT@D(12.jpg)
<h2>想要通过selenium实现自动化登录功能想要下载以下插件和导入


```bash
#导包
from selenium import webdriver
import time
from selenium.webdriver import ActionChains
import requests
import base64
import urllib
import cv2

下载chromedriver地址:http://npm.taobao.org/mirrors/chromedriver/
还需要下载Google Chrome,Google Chrome的版本要和chromedriver的版本相似

```

 - 先获取到token
```bash
res = requests.get("https://aip.baidubce.com/oauth/2.0/token?grant_type=client_credentials&client_id=82xydWXnfXIBar87YkgeshLD&client_secret=uWmR1rBWwhMSqzwnZrOfZLkUsO5enXrp")

token = res.json()['access_token']

#开始智能识图

#接口地址
url = 'https://aip.baidubce.com/rest/2.0/ocr/v1/accurate_basic?access_token='+token
#定义头部信息
myheaders = {'Content-Type':'application/x-www-form-urlencoded'}

```


 - 建立浏览器对象
```bash
browser = webdriver.Chrome('D:\\Chromes\\chromedriver.exe')

#打开网址
browser.get('http://localhost:8080/Login')

time.sleep(2)

```
 - 选取验证码图片和降噪操作
 
```bash
code_images = browser.find_element_by_class_name('imgcode')   #imgcode是验证码的类选择器

#截大图
# browser.get_screenshot_as_file("md.png")

#只截取指定对象
code_images.screenshot("md.png")

#降噪
img = cv2.imread('./md.png',cv2.IMREAD_GRAYSCALE)

cv2.imwrite('./code1.png',img)

```

 - 对截取出来的图片进行读取图片和编码操作
```bash
#读取图片
myimg = open('./code1.png','rb')
temp_img = myimg.read()

# myimg.close()

#进行base64编码
temp_data = {'image':base64.b64encode(temp_img)}

#对图片地址进行urlencode操作
temp_data = urllib.parse.urlencode(temp_data)

#请求视图接口
res = requests.post(url=url,data=temp_data,headers=myheaders)

code = res.json()['words_result'][0]['words']



code = str(code).replace(' ','')
print(code)   #是为了打印一下验证码为了更清楚的知道是不是获取到验证码

```
如图所示:
![photo1](/hexo-personage/images/photo1.png)




 - 进行填入用户名和密码等一系列操作(你有那些输入框，就写几个输入框里面的东西)
 
```bash
browser.find_elements_by_tag_name('input')[1].send_keys('qiqi')  #登录的用户名

#填入密码
browser.find_elements_by_tag_name('input')[2].send_keys('1234')   #登录的密码

#填入手机号
browser.find_elements_by_tag_name('input')[3].send_keys('13209567933')   #登录的手机号

#填入验证码
browser.find_elements_by_tag_name('input')[4].send_keys(code)  #登录的图片验证码


time.sleep(2)  #等待时间

```


 - 进行定义拖动对象，拖动滑块验证码
```bash

#滑块按钮标签
button = browser.find_element_by_class_name('dv_handler')

#滑块长度标签
box = browser.find_element_by_class_name('dv_text')   #通过f12查找按钮标签的位置


#获取按钮长度
square_len = button.size.get('width')

#获取滚动条长度
box_len = box.size.get('width')

# print(square_len)
# print(box_len)

#定义动作对象
action = ActionChains(browser)

#定义动作类型
action.click_and_hold(button).perform()

#释放动作
action.reset_actions()

#拖动距离
action.move_by_offset((box_len-square_len),0).perform()


#点击登录按钮
browser.find_element_by_class_name('h-btn').click()

time.sleep(5)

browser.close()

```

登录成功如图所示:
![photo](/hexo-personage/images/photo.png)

