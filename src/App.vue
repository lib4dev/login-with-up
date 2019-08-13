<template>
  <div id="app">
    <login-with-up
      ref="LoginUp"
      :loginCallBack="call"
      :requireLabel="true"
      :requireCode="true" 
      :bgImageUrl="bgImageUrl"
      :systemName="systemName"
      :loginNameLabel="loginNameLabel"
      :loginPwdLabel="loginPwdLabel"
      :sendBtnLabel="sendBtnLabel"
      :codeLabel="codeLabel"
      :sendCode="sendWXCode"
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
        copyright:"版权",
        loginTitle:"用户登录",
        loginNameLabel:"用户名",
        loginNameHolder:"请输入用户名",
        loginPwdLabel:"密码",
        loginPwdHolder:"请输入用户密码",
        codeLabel:"微信验证码",
        codeHolder:"请输入验证码",
        sendBtnLabel:"获取微信验证码"
      }
    },
    components:{ //注册插件
      loginWithUp
    },
    created(){
      //       :requireLabel="true"
      // :requireCode="true"

      // :loginNameLabel="loginNameLabel"
      // :loginPwdLabel="loginPwdLabel"
      // :sendBtnLabel="sendBtnLabel"
      // :codeLabel="codeLabel"
      // :sendCode="sendWXCode"
      // sessionStorage.setItem("systeminfo",JSON.stringify({"name":"芯片卡运营管理系统","copyright":"版权所有"}))
    },
    methods:{
      call(e){
        //在这里获取数据进行登录
        console.log(e);
        //如果登陆失败，发送错误信息
        //this.errMsg = {message:"",timestamp:  Date.parse(new Date())};

        // this.$refs.LoginUp.success("/path")

      },
      sendWXCode(e){
        this.$refs.LoginUp.showError("验证码发送成功")
        this.$refs.LoginUp.countDown("获取微信验证码")
      },
      wxLogin(){
        window.location.href = "https://open.weixin.qq.com/connect/qrconnect?appid=wxbdc5610cc59c1631&redirect_uri=https%3A%2F%2Fpassport.yhd.com%2Fwechat%2Fcallback.do&response_type=code&scope=snsapi_login&state=3c539cf7b387374580153e2fa1873f1c#wechat_redirect";
      }
    }
  }
</script>


