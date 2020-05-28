---
title: 笔记(vue-i18n的使用,前端实现中英文切换)
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


<div align="right"><img src='http://q9hs8ny3b.bkt.clouddn.com/%5B0_%5DVOPJAZOOS%60%249IT%40D%2812.gif' type='img/gif'></div>


###首先建个文件夹，结构是这样的，为了方便以后更改方便，强烈推荐的做法。

![20190912103901155](/hexo-personage/images/20190912103901155.png)


 - 这里的en.js和zh.js就是中英文文件了，你要加其他语言继续加几文件就行。代码结构为这样,中英文结构要一致，变量名也要一样，这个你懂的

![20190912104051532](/hexo-personage/images/20190912104051532.png)


 - 再看一下index.js这个文件负责的事情就是引入这两个文件，声明文件，最后暴露接口即可，主要是为了后期的其他js文件也能用到，我学习一向是先把代码弄上去，先实现，然后再去弄懂它的知识点。
```
import Vue from 'vue';
import VueI18n from 'vue-i18n';
import Cookies from 'js-cookie';

Vue.use(VueI18n);
// 从手机app端写入cookie,然后取出来确定是什么语言。
// 我这里主要是不会去切换他，如果要切换，就i18n.locale去改。如果页面上有切换按钮就更简单了。
// Cookies.set('language', 'en');
const language = Cookies.get('language');
 
const i18n = new VueI18n({
  // 语言标识，后面会用做切换和将用户习惯存储到本地浏览器
  locale: language, //  语言标识
  messages: {
    'zh-CN': require('./config/zh'),
    en: require('./config/en')
  }
});
 
export default i18n;
```

 - 接下来就是用了，怎么用才是关键

在整个项目所有的vue页面用，这个时候要咋main.js引入

![20190912110429461](/hexo-personage/images/20190912110429461.png)

![20190912110451725](/hexo-personage/images/20190912110451725.png)



在单文件中想引入某个在字段里，html部分这样$t('en或者zh中你设置的变量名')，
```
<div slot="header">
  <van-cell-group>
    <van-cell :title="$t('tabbarHome.popular')" isLink>
      <router-link to="/items/hot" class="text-desc">{{ $t('tabbarHome.morePopular') }}
      </router-link>
    </van-cell>
  </van-cell-group>
</div>
```
 - vue文件中js部分这样子取，此时是不用引入i18n的哦，因为已经全局使用了。。也是很方便，直接this.$t('en或者zh中你设置的变量名')

![20190912111053163](/hexo-personage/images/20190912111053163.png)


还有一种情况还会使用，就是单个js文件比如，封装好的request或者router路由，没准一些错误提示要英文，所以也得用，直接看代码。
```
import I18n from '@/language';
// 要引入才能用
service.interceptors.response.use(
  response => {
    const res = response.data;
 
    if (res.errno === 501) {
      // 用在这里
      Toast.fail(I18n.t('requestErrText.pleaseLogin'));
      removeLocalData();
      setTimeout(() => {
        window.location = '#/login/';
      }, 1500);
      return Promise.reject('error');
    } else if (res.errno === 502) {
      Toast.fail(I18n.t('requestErrText.insetErr'));
      return Promise.reject('error');
    }
    if (res.errno === 401) {
      Toast.fail(I18n.t('requestErrText.paramesErr'));
      return Promise.reject('error');
    }
    if (res.errno === 402) {
      Toast.fail(I18n.t('requestErrText.keyErr'));
      return Promise.reject('error');
    } else if (res.errno !== 0) {
      // 非5xx的错误属于业务错误，留给具体页面处理
      return Promise.reject(response);
    } else {
      return response;
    }
  },
  error => {
    console.log('err' + error); // for debug1
    removeLocalData();
    Dialog.alert({
      title: I18n.t('requestErrText.warn'),
      message: I18n.t('requestErrText.outTime')
    }).then(() => {
      window.location = '#/login/';
    });
    return Promise.reject(error);
  }
);
```
这就实现所有的中英文了。。其他高级功能就看文档去吧。。只要会了前边这些，其他都是小事。


