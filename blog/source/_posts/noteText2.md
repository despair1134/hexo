---
title: 笔记(Django的基本配置)
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



<h2>一、Django基本配置
 - 1.新建app
​   - 在项目目录中，即manage.py文件所在的目录执行下面代码：

```

python manage.py startapp app

```
 - 2.在项目中添加新建的app
 
```

找到settings.py文件在INSTALLED_APPS中添加自定义的app

```

```

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app',  # 自定义APP
]

```

 - 3.设置中文和使用北京时间
​  - Django settings中的默认设置
```

LANGUAGE_CODE = 'en-us'

TIME_ZONE = 'UTC'

USE_I18N = True

USE_L10N = True

USE_TZ = True

```

​  - 更改后
```
LANGUAGE_CODE = 'zh-hans'

TIME_ZONE = 'Asia/Shanghai'

USE_I18N = True

USE_L10N = True

USE_TZ = False

```
 - 4.配置Django连接MySQL数据库
 - 在settings中修改DATABASES如下:
 
```

   'default': {
          'ENGINE': 'django.db.backends.mysql', #数据库引擎
          'NAME': 'md2',                       #数据库名
          'USER': 'root',                       #用户名
          'PASSWORD': '123456',                   #密码
          'HOST': '',                           #数据库主机，默认为localhost
          'PORT': '',                           #数据库端口，MySQL默认为3306
          'OPTIONS': {
             'autocommit': True,
         }
    }
```
​  - Django默认使用的是MySQLdb模块连接数据库，告诉DJango用pymysql这个模块去连接MySQL，在settings.py同目录下的__init__.py文件中,指定使用pymysql模块代替MySQLdb

```
import pymysql
pymysql.install_as_MySQLdb()
```
 - 5.记录models.py的改动以及将改动迁移到数据库
​  - 记录models.py的改动

```

python manage.py makemigrations

``` 

 - 将改动迁移到数据库
 
```

python manage.py migrate

```

 - 6.静态文件配置
 

```
# settings.py

STATIC_URL = '/static/'

# 新增下面内容
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static')
]

```