---
title: 笔记(vue里面实现画中画效果)
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



```vue

<td>

							<!--视频地址-->
			  <video id="video" v-show="videosrc" width="300" height="220" :src="videosrc" controls="controls" autoplay="autoplay" muted >

			  </video>

              <br />
        <Button color="red" @click="changepic" >{{mybutton}}

        </Button>
						</td>


//视频播放地址
      videosrc:'',
         //切换按钮变量
    mybutton:"进入画中画",



 mounted:function(){


  			//视频地址
          this.videosrc = "http://q9hs8ny3b.bkt.clouddn.com/"+result.data.img;
          // http://q9hs8ny3b.bkt.clouddn.com/FgbDRDq6by75qe6c9TMTL4YgedUE

  			});

methods: {

      //定义画中画切换
      changepic:function(){
        // 判断是否进入画中画
        if (video !== document.pictureInPictureElement) {
          // 进入画中画
          video.requestPictureInPicture();
          this.mybutton = '退出画中画'
        } else {
          // 退出画中画
          document.exitPictureInPicture();
          this.mybutton = '进入画中画'
        }
      },

  	upload_qiniu:function(e){

  		//获取文件
  		let file = e.target.files[0];

  		//声明表单参数
  		let param = new FormData();

  		param.append('file',file,file.name);
  		param.append('token',this.token);

  		//自定义axios
  		const axios_qiniu = this.axios.create({withCredentials:false});

  		//发送请求
  		axios_qiniu({
  			method:'POST',
  			url:'http://up-z1.qiniup.com/',
  			data:param,
  			timeout:30000,
          //上传过程中的方法
          onUploadProgress:(e)=>{

  			    //计算上传百分比
              var complete = (e.loaded / e.total);

              //处理美化
              if(complete < 1){
                  this.load_percent = (complete * 100).toFixed(2) + '%';

                  this.load_int = parseInt((complete * 100).toFixed(2));
              }
          }
  		}).then(result =>{

  		    //手动赋值100%
          this.load_percent = '100%';

          this.load_int = 100;

  			console.log(result);
  			//图片地址
  			this.src = "http://q9hs8ny3b.bkt.clouddn.com/"+result.data.key;

  			//视频地址
  			this.videosrc = "http://q9hs8ny3b.bkt.clouddn.com/"+result.data.key;
  			// http://q9hs8ny3b.bkt.clouddn.com/   上传视频地址

          //请求后台接口
  		this.axios.get('http://localhost:8000/updateuser/',{params:{uid:localStorage.getItem("uid"),img:result.data.key}}).then((result) =>{


  			console.log(result);



  		});


```


