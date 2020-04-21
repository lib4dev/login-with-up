<template>
  <div id="app">
    <login-with-up
      ref="LoginUp"
      :loginCallBack="login"
      :systemName="systemName"
      :requireCode="false" 
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
      :loginNameValue="loginNameValue"
      :loginPwdValue="loginPwdValue"

      :subMainW3="subMainW3"
      :subMainW3Form="subMainW3Form"
      :subMainW3Title="subMainW3Title"
      :subMainW3TitleIcon="subMainW3TitleIcon"
      :subMainW3Label="subMainW3Label"
      :subMainW3LabelIcon="subMainW3LabelIcon"
      :subMainW3LabelInput="subMainW3LabelInput"
      :subMainW3Tips="subMainW3Tips"
      :subMainW3TipsIcon="subMainW3TipsIcon"
      :subMainW3Submit="subMainW3Submit"
      :subMainW3Pwd="subMainW3Pwd"
      :subMainW3Footer="subMainW3Footer"

      :forgetPwdCallBack="forgetPwd"
      :registerCallBack="register"
    >
    </login-with-up>
  </div>
</template>

<script>
  import loginWithUp from '../src/plugin/loginWithUp/login-with-up'; // 引入

  import headImg from './assets/back-bg.jpg'
  export default {
    name: 'app',
    data () {
      return { //https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1565240473656&di=d7802b33660d2ae5326216421348b0e5&imgtype=0&src=http%3A%2F%2Fg.hiphotos.baidu.com%2Fimage%2Fpic%2Fitem%2F5366d0160924ab18014cefd83bfae6cd7a890b82.jpg
        bgImageUrl: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1565240473656&di=d7802b33660d2ae5326216421348b0e5&imgtype=0&src=http%3A%2F%2Fg.hiphotos.baidu.com%2Fimage%2Fpic%2Fitem%2F5366d0160924ab18014cefd83bfae6cd7a890b82.jpg",
        systemName:"", //能源业务中心运营管理系统
        copyright:"版权信息555",
        loginTitle:"加油卡回收商户系统",
        loginNameLabel:"用户名",
        loginNameHolder:"请输入管理员用户名",
        loginPwdLabel:"密码",
        loginPwdHolder:"请输入管理员用户密码",
        codeLabel:"微信验证码",
        codeHolder:"请输入微信验证码",
        sendBtnLabel:"获取短信验证码",
        loginNameValue: "13125896369",
        loginPwdValue: "132dasdasdasd",

        subMainW3:　"justify-content: flex-end;",   //外层样式
        subMainW3Form:"margin: 0 50px;background: #dcdcdc;border-radius: 10px;border-bottom: 0px;width:28%;",　//form样式
        subMainW3Title:"font-size: 30px:color: blue;font-weight: bold;",
        subMainW3TitleIcon:"display: none;",
        subMainW3Label:"color: #ff3333;",
        subMainW3LabelIcon:"color: blue;",
        subMainW3LabelInput:"border: 1px #ff3434 solid;",
        subMainW3Tips:"color: blue;",
        subMainW3TipsIcon:"color: red;",
        subMainW3Submit:"width: 71%;border-radius: 10px;background: #26d8a4;",
        subMainW3Pwd:"font-size: 14px;color: #ff3234;",
        subMainW3Footer:""
      }
    },
    components:{ //注册插件
      loginWithUp
    },
    created(){
      this.bgImageUrl = headImg
    },
    methods:{
      login(e){
        //在这里获取数据进行登录
        console.log(e);
        //如果登陆失败，发送错误信息
        this.$refs.LoginUp.showError("登录失败")
        // this.$refs.LoginUp.success("/path")

        this.$refs.LoginUp.reSendCode()
      },
      sendCode(e){
        this.$refs.LoginUp.showError("验证码发送成功")
        this.$refs.LoginUp.countDown()
      },
      forgetPwd(){
        alert("点击了忘记密码")
      },
      register(){
        alert("点击了注册")
      }
    }
  }
</script>

<style scoped>
.login-border{
  background: red;
  visibility: hidden;
  display: flex;
  justify-content: space-between;
}
</style>


