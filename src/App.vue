<template>
  <div id="app">
    <login-with-up
      :host="host"
      :sysname="sys_name"
      :copyright="copyright"
      :ident="ident"
      v-on:loginCall="call"></login-with-up>
  </div>
</template>

<script>
  import loginWithUp from '../src/plugin/loginWithUp/login-with-up'; // 引入
  export default {
    name: 'app',
    data () {
      return {
        host:"http://api.sso2.100bm.cn:6688/sso/login",
        sys_name: "电子优惠券系统",
        copyright:"四川千行你我科技有限公司Copyright© 2018 版权所有",
        ident:"sso"
      }
    },
    components:{ //注册插件
      loginWithUp
    },
    methods:{
      call(e){
        console.log(e)
        let d = sessionStorage.getItem("__jwt__")
        console.log(d)
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
  }
</style>
