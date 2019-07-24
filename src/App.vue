<template>
  <div id="app">
    <login-with-up
      :call="call"
      :err-msg.sync="errMsg"
      :getCodeCall="getCodeCall"
      :wxlg="wxLogin"
      ref="LoginUp"
    >
    </login-with-up>
  </div>
</template>

<script>
  import loginWithUp from '../src/plugin/loginWithUp/login-with-up'; // 引入
  export default {
    name: 'app',
    data () {
      return {
        errMsg:{}
      }
    },
    components:{ //注册插件
      loginWithUp
    },
    created(){
      sessionStorage.setItem("systeminfo",JSON.stringify({"name":"芯片卡运营管理系统","copyright":"版权所有"}))
    },
    methods:{
      call(e){
        //在这里获取数据进行登录
        console.log(e);
        //如果登陆失败，发送错误信息
        //this.errMsg = {message:"",timestamp:  Date.parse(new Date())};

        this.$refs.LoginUp.success("/path")

      },
      getCodeCall(e){
        this.errMsg = {message:"发送中。。。。",timestamp: Date.parse(new Date())};
      },
      wxLogin(){
        window.location.href = "https://open.weixin.qq.com/connect/qrconnect?appid=wxbdc5610cc59c1631&redirect_uri=https%3A%2F%2Fpassport.yhd.com%2Fwechat%2Fcallback.do&response_type=code&scope=snsapi_login&state=3c539cf7b387374580153e2fa1873f1c#wechat_redirect";
      }
    }
  }
</script>


