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
      :copyright="copyright"
      :systemName="systemName"
      @loginCall="call"
      ref="loginItem"
    >

    </login-with-up>
  </div>
</template>

<script>
  import loginWithUp from 'login-with-up'; // 引入
  export default {
    name: 'app',
    data () {
      return {
        systemName: "电子优惠券系统",
        copyright:"四川千行你我科技有限公司Copyright© 2018 版权所有",
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
        this.$refs.loginItem.showMsg("登录中。。。。");
      }
    }
  }
</script>
```


![预览](https://raw.githubusercontent.com/micro-plat/login-with-up/dev/view.png)
