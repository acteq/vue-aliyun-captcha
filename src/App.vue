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

            <aliyun-capcha appkey='FFFF00000000016C4A73' scene='login' v-on:callback='onCaptcha'></aliyun-capcha>
            
        </form>
    </div>
</template>

<script>
import Vue from 'vue'
import AliyunCaptcha from './components/AliyunCaptcha'
export default {
  name: 'app',
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

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 20px;
  margin-right: 20px;
}

</style>
