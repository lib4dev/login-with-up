# login-with-up

> vue 用户登录组件

## 1.install

``` bash
npm i login-with-up --save

```

## 2.use

``` js
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
  import loginWithUp from 'login-with-up'; // 引入
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
```

### 3.Attributes

| 参数            | 说明         | 类型 | 可选值  |    默认值    | 
| ----------    | ------------ | :----: | :---: | :--------: | 
| system-name   | 登录系统名称   | string  |  -   | 登录组件  | 
| copyright     | 版权信息      | string   |  -   |  千行你我版权所有   | 
| err-msg       | 错误提示信息  |   object  |  -   | {message:"",timestamp:""} | 
| conf          | 输入框配置    |   object  |  -   | {loginNameType:"请输入邮箱或用户名",pwd:"输入密码", validateCode:"输入微信验证码"} | 
| require-code  | 是否开启验证码 |   boolean |  -   |  false     | 
| call          | 登录回调   | function(val) 用于登录 |       |    -     |
| getCodeCall   | 获取验证码回调 | function(val) 用于获取验证码 |   |  -     |




![预览](https://raw.githubusercontent.com/micro-plat/login-with-up/dev/view.png)
