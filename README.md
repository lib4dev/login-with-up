# login-with-up

> vue 用户登录组件

## install

``` bash
npm i login-with-up --save

```

## use

```vue
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
  import loginWithUp from 'login-with-up'; // 引入
  export default {
    name: 'app',
    data () {
      return {
        host:"http://api.test.cn/login",
        sys_name: "用户权限系统",
        copyright:"版权信息",
        ident:"sso"   //系统英文名称
      }
    },
    components:{ //注册插件
      loginWithUp
    },
    methods:{
      call(e){
        console.log(e)
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
```


![预览](https://raw.githubusercontent.com/micro-plat/login-with-up/dev/view.png)
