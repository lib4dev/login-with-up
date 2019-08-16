<template>
  <div id="app">
    <login-with-up
      ref="LoginUp"
      :loginCallBack="login"
      :systemName="systemName"
      :requireCode="true" 
      :loginTitle="loginTitle"
      :loginNameLabel="loginNameLabel"
      :loginNameHolder="loginNameHolder"
      :loginPwdLabel="loginPwdLabel"
      :loginPwdHolder="loginPwdHolder"
      :codeLabel="codeLabel"
      :codeHolder="codeHolder"
      :sendBtnLabel="sendBtnLabel"
      :sendCode="sendCode"
      :bgImageUrl="bgImageUrl"
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
        bgImageUrl: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1565240473656&di=d7802b33660d2ae5326216421348b0e5&imgtype=0&src=http%3A%2F%2Fg.hiphotos.baidu.com%2Fimage%2Fpic%2Fitem%2F5366d0160924ab18014cefd83bfae6cd7a890b82.jpg",
        systemName:"能源业务中心运营管理系统",
        copyright:"版权信息555",
        loginTitle:"管理员登录",
        loginNameLabel:"管理员用户名",
        loginNameHolder:"请输入管理员用户名",
        loginPwdLabel:"管理员密码",
        loginPwdHolder:"请输入管理员用户密码",
        codeLabel:"微信验证码",
        codeHolder:"请输入微信验证码",
        sendBtnLabel:"获取微信验证码",
      }
    },
    components:{ //注册插件
      loginWithUp
    },
    created(){

    },
    methods:{
      login(e){
        //在这里获取数据进行登录
        console.log(e);
        //如果登陆失败，发送错误信息
        this.$refs.LoginUp.showError("登录失败")
        // this.$refs.LoginUp.success("/path")
      },
      sendCode(e){
        this.$refs.LoginUp.showError("验证码发送成功")
        this.$refs.LoginUp.countDown("获取微信验证码")
      }
    }
  }
</script>


