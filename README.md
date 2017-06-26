## 介绍

阿里云滑动验证vue 2。0组件


## 使用

```
<template>
    <div id="app">
        <form novalidate @submit.stop.prevent="submit">
            <div>
                <label>登录名</label>
                <input type="text" v-model="username"></md-input>
            </div>
            <div>
                <label>密码</label>
                <input type="password" v-model="password"></md-input>
            </div>
            <button type="button" @click="onSignin">登录</button>
            <aliyun-capcha appkey='FFFF00000000016C4A73' scene='login' v-on:callback='onCaptcha'></aliyun-capcha>
            
        </form>
    </div>
</template>

<script>

import Vue from 'vue'
import AliyunCaptcha from 'vue-aliyun-captcha'
export default {
  data () {
    return {
      username:''
      ,password:''
    }
  },
  components: {
    'aliyun-capcha': AliyunCaptcha
  },
  methods: {
    onCaptcha (data) {
        this.csessionid = data.csessionid
        this.sig = data.sig
        this.token = data.token
        this.scene = data.scene
        this.nc = data.nc
    },
    onSignin () {
      
      if(this.nc) {
              this.nc.reset()
          }
      // let user = {password: this.password
      //     , session: this.csessionid
      //     , sig: this.sig
      //     , token: this.token
      //     , scene: this.scene
      // };

      // }).catch( err => {
      //     if(this.nc) {
      //         this.nc.reset()
      //     }
      // })
    }
  }
}
</script>

<aliyun-capcha appkey='aliyun 滑动验证提供的key' scene='场景' v-on:callback='onCaptcha'></aliyun-capcha>
场景为 login或register，组件会检查是否浏览器类型，在手机浏览器上，会自动加上_h5后缀，以和阿里云的验证要求一致。
callback函数原型为
onCaptcha (data) {
    this.csessionid = data.csessionid  
    this.sig = data.sig
    this.token = data.token
    this.scene = data.scene
    this.nc = data.nc  // nc 指向阿里云滑动验证码提供的对象，以便在登录／注册失败时 reset
}

另外，目前需要在html页面引入阿里云滑动验证的css
<link type="text/css" href="//g.alicdn.com/sd/ncpc/nc.css?t=1497440454594" rel="stylesheet" />
待找到好的办法后，去掉这个css引用

```
## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report


```
