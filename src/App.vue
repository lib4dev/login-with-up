<template>
  <div id="app">
    <login-with-up
      :call="call"
      :err-msg.sync="errMsg"
      :getCodeCall="getCodeCall"
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
    methods:{
      call(e){
        //在这里获取数据进行登录
        console.log(e);
        //如果登陆失败，发送错误信息
        this.errMsg = {message:"登录中。。。。",timestamp:  Date.parse(new Date())};
      },
      getCodeCall(e){
        this.errMsg = {message:"发送中。。。。",timestamp:  Date.parse(new Date())};
      }
    }
  }
</script>

