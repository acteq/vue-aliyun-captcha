<template>
    <div> 
        <div id="_umfp" style="display:inline;width:1px;height:1px;overflow:hidden"></div>  
        <div class="ln">
            <div v-bind:id="dynamicId"></div>
        </div>  
    </div>
</template>

<script>
import Vue from 'vue'
import VueScript2 from 'vue-script2'
import bowser from 'bowser'

function randomCode(length){
    let str = ''
    for (; str.length < length; str += Math.random().toString(36).substr(2)) {}
    return str.substr(0, length)
}

export default {
  name: 'vue-aliyun-captcha',
  props: {
    scene: {
      type: String,
      required: true
    },
    appkey : {
      type: String,
      required: true
    }
  },
  data () {
    return {
    }
  },
  computed: {
    dynamicId () {
      this.nc_id = randomCode(10)
      return this.nc_id
    }
  },
  
    mounted () {
        console.log(this.nc_id)
        // console.log(this.$el.lastChild())
        // console.log(this.$el.lastChild().firstChild())
        // ele.id = nc_id
        let nc = null 
        let nc_scene = bowser.mobile ? this.scene + '_h5' : this.scene;  //场景,不可更改
        let nc_token = [this.appkey, (new Date()).getTime(), Math.random()].join(':');
        let nc_option = {
            renderTo: '#' + this.nc_id,//渲染到该DOM ID指定的Div位置
            appkey: this.appkey,
            scene: nc_scene,
            token: nc_token,
            callback:  (data) => {// 校验成功回调
                this.$emit('callback', {csessionid: data.csessionid, sig: data.sig, token: nc_token, scene: nc_scene, nc: nc})
            },
            error: function (s) {
                console.log('error', s)
            },
            verifycallback: function (data) {
                if (data.code == "200") {
                }
            }
        }
        if(bowser.mobile){
            VueScript2.load('//g.alicdn.com/sd/nch5/index.js?t=1497436353263')
                .then(function () {
                    NoCaptcha.init(nc_option)
                    NoCaptcha.setEnabled(true)
                    nc = NoCaptcha
                })
        }else{
            VueScript2.load('//g.alicdn.com/sd/ncpc/nc.js?t=1497440454594')
                .then( ()=> {
                    // 
                    nc = new noCaptcha();  
                    nc.init(nc_option);
                })
        }
    }
  
}
</script>

<style scoped>
.ln {
    padding: 10px 20px;
}
.ln .h {
    display: inline-block;
    width: 4em;
}
.ln input {
    border: solid 1px #999;
    padding: 5px 8px;
}


</style>