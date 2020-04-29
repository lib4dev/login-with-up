<template>
  <div class="login-with-up">
    <div id="bg" style=" background:#2196f3" v-if="!bgImageUrl"  >
      <canvas></canvas>
      <canvas></canvas>
      <canvas></canvas>
    </div>
    <div id="bg" v-if="bgImageUrl">
      <img style="width:100%;height:100%;" :src="bgImageUrl"/>
    </div>
    <div>
      <h1 class="visible-lg-block visible-md-block sub-main-w3-sysname" v-if="systemName">{{systemName}}</h1>
      <h1 class="visible-lg-block visible-md-block sub-main-w3-sysname" style="visibility: hidden;" v-if="!systemName">{{defaultSystemName}}</h1>
      <div class="sub-main-w3">

        <form action="#" @submit.prevent="loginNow" class="sub-main-w3-form">
          <h2 class="sub-main-w3-title">{{loginTitle}}
            <i class="sub-main-w3-title-icon iconfont icon-jiantouarrow511"></i>
          </h2>

          <div class="form-style-agile" style="margin-bottom:0px !important">
            <div class="form-style-agile">
              <label v-show="requireLabel" class="sub-main-w3-label">
                <i class="sub-main-w3-label-icon iconfont icon-yonghu"></i>
                {{loginNameLabel}}
              </label>
              <input :placeholder="loginNameHolder" maxlength="32" v-model="login.username" type="text" class="sub-main-w3-label-input">
            </div>
            <div class="form-style-agile">
              <label v-show="requireLabel" class="sub-main-w3-label">
                <i class="sub-main-w3-label-icon iconfont icon-xiugaimima"></i>
                {{loginPwdLabel}}
              </label>
              <input :placeholder="loginPwdHolder" maxlength="32" v-model="login.password" type="password" class="sub-main-w3-label-input">
            </div>

            <div class="form-style-agile qx-code-ico" v-if="requireCode">
              <label v-show="requireLabel" class="sub-main-w3-label">
                <i class="sub-main-w3-label-icon iconfont icon-duanxinyanzhengma"></i>
                {{codeLabel}}
              </label>
              <div class="from-style-code">
                <input :placeholder="codeHolder" maxlength="10" v-model="login.wxcode" type="text" class="sub-main-w3-label-code-input">
                <input type="button" class="submit sub-main-w3-code-submit" @click="getValidateCode" :value="sendBtnText">
              </div>
            </div>
          </div>

          <div class="pwd-oper sub-main-w3-oper" v-if="requireOper">
            <div class="pwd-oper-item sub-main-w3-oper-left" @click="gotoPwd">{{forgetPwd}}</div>
            <div class="pwd-oper-item sub-main-w3-oper-right" @click="gotoRegister">{{userRegister}}</div>
          </div>

          <!-- checkbox -->
          <div class="wthree-text">
            <ul>
              <li>
                <span v-if="message　&& message == '请输入用户名和密码.'" style="visibility:hidden;">{{message}}</span>
                <label v-if="message　&& message != '请输入用户名和密码.'">
                <span class="sub-main-w3-tips">
                  <i class="sub-main-w3-tips-icon iconfont icon-tishi" style="font-size: 14px;"></i>
                  {{message}}
                </span>
                </label>
              </li>
            </ul>
          </div>
          <input type="submit" class="submit sub-main-w3-submit" value="立即登录">
        </form>
      </div>
      <div class="footer sub-main-w3-footer">
        <p>{{copyright}}</p>
      </div>
    </div>
  </div>
</template>

<script>
  import {drawDynamicsBG} from "../../assets/js/bg";
  export default {
    name: "loginWithUp",
    props: {
      bgImageUrl:{　//背景图片地址
        type:String,
        default:""
      },
      systemName:  { //系统名称
        type:String,
        default:"后台运营管理系统"
      },
      copyright:  {//版权信息
        type:String,
        default:""
      },
      loginTitle:{　//登录标题
        type:String,
        default: "用户登录"
      },
      loginNameLabel:{　//登录用户名标签
        type:String,
        default: "用户名"
      },
      loginNameHolder:{　//用户名输入框提示
        type:String,
        default:"请输入邮箱、手机号或用户名"
      },
      loginPwdLabel:{　//密码框标签　
        type:String,
        default:"密码"
      },
      loginPwdHolder:{　//密码输入框提示
        type:String,
        default:"请输入密码"
      },
      codeLabel:{　//微信验证码标签
        type:String,
        default:"验证码"
      },
      codeHolder:{　//微信验证码提示
        type:String,
        default:"请输入验证码"
      },
      requireCode:{ //是否有微信验证码
        type: Boolean,
        default:false,
      },
      requireLabel:{ //是否有微信验证码
        type: Boolean,
        default:true,
      },
      loginCallBack:{     //登錄的回調
        type:Function,
      },
      sendCode:{   //獲取驗證碼的回調
        type:Function,
        default:function () {
        }
      },
      sendBtnLabel:{　//获取验证码按钮文字
        type:String,
        default:"获取验证码"
      },
      requireOper:{ //是否采用默认登录页面
        type: Boolean,
        default: true,
      },

      forgetPwd:{
        type:String,
        default: "忘记密码"
      },
      userRegister:{
        type:String,
        default: "注册账号"
      },
      forgetPwdCallBack:{     //忘记密码的回調
        type:Function,
      },
      registerCallBack:{     //忘记密码的回調
        type:Function,
      },
    },
    data() {
      return {
        login: {
          username: "",
          password: "",
          wxcode: ""
        },
        defaultSystemName: "后台运营管理系统",
        message:"请输入用户名和密码.",
        sendBtnText: this.sendBtnLabel,
        totalTime: 60,
        canClick: true, //添加canClick
        clock: {},
      };
    },
    mounted() {
      if(this.bgImageUrl){
        return
      }
      drawDynamicsBG("bg");
    },
    watch: {
     
    },
    computed:{
      
    },
    methods: {
      loginNow() {
        if (!this.login.username || !this.login.password){
          this.message = "请填写用户名和密码";
          return
        }

        if(this.requireCode){
          if(!this.login.wxcode){
            this.message = "请填写微信验证码";
            return
          }
        }
        this.message = "登录中...";
        this.loginCallBack(this.login);
      },
      getValidateCode(){
        if(!this.login.username){
          this.message = "请填写用户名";
          return
        }
        if(!this.canClick){
          return
        }
        this.sendCode(this.login);
      },
      success(path){
        window.location.href = path;
      },
      showError(msg){
        this.message = msg
      },
      setLoginAccount(account){
        this.login.username = account
      },
      countDown() {
          if (!this.canClick) return
          this.canClick = false
          this.sendBtnText = this.totalTime + 's后重新获取'
          this.clock = window.setInterval(() => {
              this.totalTime--
              this.sendBtnText = this.totalTime + 's后重新获取'
              if (this.totalTime < 0) {
                  window.clearInterval(this.clock)
                  this.sendBtnText = this.sendBtnLabel
                  this.message = '请输入用户名和密码.'
                  this.totalTime = 60
                  this.canClick = true //这里重新开启
              }
          }, 1000)
      },
      reSendCode(){
        window.clearInterval(this.clock)
        this.sendBtnText = this.sendBtnLabel
        this.message = '请输入用户名和密码.'
        this.totalTime = 60
        this.canClick = true //这里重新开启
        this.login.wxcode = ""
      },
      gotoPwd(){
        this.forgetPwdCallBack()
      },
      gotoRegister(){
        this.registerCallBack()
      }
    }
  };
</script>

<style scoped>

@font-face {font-family: "iconfont";
  src: url('//at.alicdn.com/t/font_889924_v8kg8xtspv.eot?t=1550461337444'); /* IE9 */
  src: url('//at.alicdn.com/t/font_889924_v8kg8xtspv.eot?t=1550461337444#iefix') format('embedded-opentype'), /* IE6-IE8 */
  url('data:application/x-font-woff2;charset=utf-8;base64,d09GMgABAAAAAAWcAAsAAAAACtQAAAVQAAEAAAAAAAAAAAAAAAAAAAAAAAAAAAAAHEIGVgCDXgqIEIZfATYCJAMcCxAABCAFhG0HdBtLCRGVpA+R/Uww8Wj0WGn+ZLvzVNBoJ3Ls8/ynnOn7M5IsywCwRAEGQwhAToreQhjo1LKbEp3VAAEEdNdbEyhR97+UKx37aIasaVlKQiQ0W2tWBmEiVMtPljX6zDULlB7gwbHUTw+gvA9+l47aRsNKoAFFJEaCTn9CE+U5gG3i/dbdQAAUhFEOatOuSwIyBOgHEQCaMHb0UMgJHYQKPEFWSgVnCqBlECGzT9kjAF96v0+/ohbJAIPIod+rx6i2I9AiF8id56qmYgBXoQ5nAcCeB3AA5QAEgD4qtKngQawcHIq8vYAEADNkMEhDjTeSq51rmBubO69pJNv1mQSz8Q8eIIEggkMAA0gCZEYK2ycuAbmAxYvkEWhAkNeGBiLkDaEBh3wsNBAgP48aMACAzjnDDMAJUAuADkNXlHvMHAwC+HOKRr01ajWZbOQ1Kj69XnaYQrH5r35IvP4x+fan9JvcnDM3Mjerr6vX+ptp+cqeG9Qv/hxFht3XMR9KtsmhV0B67WpnYlXWnVqT8ax1JfM8ypq3aeOPBppXrXJi9WqXZe1aosya+a9fJ1+9Srx5k3r7Nk2yZoCBqcamVynl1ZswVbzWfBq35kB2n3MMrVp/eLfHZaPVBwMbM+5pmavVl8HUS9krlNxzBWzWkz9boV44fd0GyqlLQbb5cl3L2asRvuXKiDNXlv2Cl/obaefb9rdfx+YTMyZSRj7ZuuuSl62/XGBKpk17rwb4hisTlD3+jZu6tg/rsv7h1Wq+kqWMp+xm2UbaUJj1rivb9m36dVJNFA9Xi8+HZn5cfGNRtWu9YW9gS/6ezN0tNZCrEDvLo1x9J0aj4juD7XBsUfQtppWLa/zd+a8Of3X+O9S4wLbMuMxWkDe40LosscxaiPrfm+d81fPAdXt7a2pBjWHVbS/+l6fk4Wl2yd522PwaZjMVez9q3KZrtyXoObq/ekDOygdU7+gL+qz+AiLDWw5h6kIaRIMd6xA2mMINpkELGsyQe1RCubKA7d7J5jnKd7sWkBSm3Y5KB8dOv9kCBjQb0hU733T7rvyfZoffY3RFja0rMIptn6ouLVzv/9uHWvCVOxkNv95Wu14tY+va26C5ypLUd/acfkSwcW3qmyyrofUOcA1RQ+seEDS7qIb29Re20AYvkpO6Tp2mTLG20KEBU3HNMqU+trFc05pjW23lmjKQc3HzxbGflX3jHOp2DanZQ7or1/SGXMFKdbbfK49NP2923VLcy71lrkX1yO076jpx3qawafu+jZblnViql9r2EDryA/kZ/dH37hcVNXacLgj4hzhuHDHU8C5rKbXiaAYA0P7hk5sB7TL/iLcUSN/zvzkTrvFUAcCvsWs/6r1wpnbvvwZbmvwvm//5infLOq33ajRVAFdF+GXK0Zj/E+Y3nCAoQl2vm9Tb2hcE0BmlDy9cohjTgB+uy+4Np3A1LgSIzBUBoxMCTpaPFLDlIDKoAxJZc1CU8ecbODRdND2AUosAYrWBMbsKnNVBCtgLIrcPEmtdUKTaXtGgMD15TQgzukGooUQchGnMJn7xN8ncYRIXesR/SlIdBHKYTn5SoGSIY3ohTyMTIAl7YOJ+5BxDTdhQlO1ylt1fAKTojXaRfWuitQRBGXIDghooIiwQztyZSWb834iUc1BS0VVm/UcSKb15QBxsgPyUhUZdl1LZFaSTQUYEIGwh8wCTFuKMGAO1eDeDRDI7uUPk7gWQU0lT2W56hb/PGwAU+hvrYsRJIJEk0pEM8TpVemk0hoxzTBIur0Sq5iBVPigpP+lK5xK1N7bAdcoxVDrUTnWjKEiPrRY=') format('woff2'),
  url('//at.alicdn.com/t/font_889924_v8kg8xtspv.woff?t=1550461337444') format('woff'),
  url('//at.alicdn.com/t/font_889924_v8kg8xtspv.ttf?t=1550461337444') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+ */
  url('//at.alicdn.com/t/font_889924_v8kg8xtspv.svg?t=1550461337444#iconfont') format('svg'); /* iOS 4.1- */
}

.iconfont {
  font-family: "iconfont" !important;
  font-size: 16px;
  font-style: normal;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

.icon-tishi:before {
  content: "\e61f";
}

.icon-jiantouarrow511:before {
  content: "\e6b9";
}

.icon-yonghu:before {
  content: "\e639";
}

.icon-tishi2:before {
  content: "\e669";
}

.icon-xiugaimima:before {
  content: "\e633";
}

.icon-duanxinyanzhengma:before {
  content: "\e61b";
}


.qx-code-ico .icon-duanxinyanzhengma{ font-size: 21px; position: relative; top:3px;}

  .wthree-text li {
    height: 18px;
    line-height: 18px;
  }

  .wthree-text span {
    color: #f7296f;
    font-weight: 600;
  }


  .scan_code {
    position: absolute;
    float: right;
    right: 5%;
    top: 5%;
  }

  .scan_code_icon_text {
    display: none;
  }

  .scan_code_icon img {
    opacity: 0.5;
    filter: alpha(opacity=50);
  }

  .scan_code:hover .scan_code_icon_text {
    display: inline-block;
  }

  .scan_code a:hover img {
    opacity: 1;
    filter: alpha(opacity=100);
  }

  .code-center {
    display: flex;
    justify-content: center;
  }

  ol,
  ul {
    list-style: none;
    margin: 0px;
    padding: 0px;
  }

  blockquote,
  q {
    quotes: none;
  }

  blockquote:before,
  blockquote:after,
  q:before,
  q:after {
    content: "";
    content: none;
  }

  table {
    border-collapse: collapse;
    border-spacing: 0;
  }

  /* start editing from here */

  a {
    text-decoration: none;
  }

  .txt-rt {
    text-align: right;
  }

  /* text align right */

  .txt-lt {
    text-align: left;
  }

  /* text align left */

  .txt-center {
    text-align: center;
  }

  /* text align center */

  .float-rt {
    float: right;
  }

  /* float right */

  .float-lt {
    float: left;
  }

  /* float left */

  .clear {
    clear: both;
  }

  /* clear float */

  .pos-relative {
    position: relative;
  }

  /* Position Relative */

  .pos-absolute {
    position: absolute;
  }

  /* Position Absolute */

  .vertical-base {
    vertical-align: baseline;
  }

  /* vertical align baseline */

  .vertical-top {
    vertical-align: top;
  }

  /* vertical align top */

  nav.vertical ul li {
    display: block;
  }

  /* vertical menu */

  nav.horizontal ul li {
    display: inline-block;
  }

  /* horizontal menu */

  img {
    max-width: 100%;
  }

  /*end reset*/

  /* body {
    font-family: "Open Sans", sans-serif;
    font-size: 100%;
    background: #2196f3;
  } */

  /*--header--*/

  h1 {
    font-size: 3.5em;
    color: #fff;
    letter-spacing: 3px;
    text-align: center;
    margin: 3vw 1vw;
    font-family: "Josefin Sans", sans-serif;
  }

  h1 span,
  h2 i {
    color: #f7296f;
  }

  h2 {
    color: #fff;
    font-size: 22px;
    font-weight: 500;
    letter-spacing: 1px;
    text-transform: capitalize;
    margin-bottom: 1em;
  }

  /*--//header--*/

  /*-- content --*/
  .wechat-code{
    width: 230px;
    height: 230px;
    margin-top: 20px;
    margin-left: 124px;
  }

  .sub-main-w3 {
    display: -webkit-flex;
    display: -webkit-box;
    display: -moz-flex;
    display: -moz-box;
    display: -ms-flexbox;
    display: flex;
    align-items: center;
    -webkit-box-pack: center;
    -moz-box-pack: center;
    -ms-flex-pack: center;
    -webkit-justify-content: center;
    justify-content: center;
  }

  .sub-main-w3 form {
    max-width: 600px;
    margin: 0 5vw;
    background: rgba(10, 10, 10, 0.17);
    padding: 3.5vw;
    box-sizing: border-box;
    display: -webkit-flex;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    border-bottom: 8px solid #f7296f;
    border-radius: 30px 30px 50px 50px;
    position: relative;
  }

  .form-style-agile {
    margin-bottom: 1.5em;
    flex-basis: 100%;
    -webkit-flex-basis: 100%;
    text-align: left;
  }

  .from-style-code {
    display: flex;
    flex-direction: row;
  }

  .from-style-code input[type="text"]{
    width: 60% !important;
  }

  .from-style-code input[type="button"]{
    color: #fff;
    background: #f7296f;
    border: none;
    padding: 13px 0;
    outline: none;
    width: 26%;
    font-size: 15px;
    cursor: pointer;
    letter-spacing: 2px;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
    box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.49);
    position: absolute;
    right: 65px;
  }

  .from-style-code input[type="button"]:hover {
    background: #000;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
  }

  .center {
    text-align: center;
  }

  .sub-main-w3 label {
    font-size: 14px;
    color: #fff;
    display: inline-block;
    font-weight: 500;
    margin-bottom: 12px;
    text-transform: capitalize;
    letter-spacing: 1px;
  }

  .sub-main-w3 label i {
    font-size: 16px;
    margin-left: 5px;
    color: #f7296f;
    border-radius: 50%;
    line-height: 1.9;
    text-align: center;
  }

  .form-style-agile input[type="text"],
  .form-style-agile input[type="password"] {
    width: 100%;
    color: #000;
    outline: none;
    font-size: 14px;
    letter-spacing: 1px;
    padding: 15px 15px;
    box-sizing: border-box;
    border: none;
    border: 1px solid #b4b4b4;
    background: #fff;
  }

  .sub-main-w3 input[type="submit"] {
    color: #fff;
    background: #f7296f;
    border: none;
    padding: 13px 0;
    margin-top: 30px;
    outline: none;
    width: 36%;
    font-size: 16px;
    cursor: pointer;
    letter-spacing: 2px;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
    box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.49);
  }

  .sub-main-w3 input[type="submit"]:hover {
    background: #000;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
  }

  .wthree-text {
    width: 100%;
  }

  .wthree-text ul li:nth-child(1) {
    float: left;
  }

  .wthree-text ul li:nth-child(2) {
    float: right;
  }

  .wthree-text ul li {
    display: inline-block;
  }

  .wthree-text ul li a {
    color: #fff;
    font-size: 14px;
    letter-spacing: 1px;
    font-weight: 500;
  }

  /*-- checkbox --*/

  .wthree-text label {
    font-size: 14px;
    color: #fff;
    cursor: pointer;
    position: relative;
  }

  .wthree-text {
    text-align: center;
  }

  input.invalid {
  }

  input.checkbox {
    background: #f7296f;
    cursor: pointer;
    width: 1.2em;
    height: 1.2em;
    vertical-align: text-bottom;
  }

  input.checkbox:before {
    content: "";
    position: absolute;
    width: 1.2em;
    height: 1.2em;
    background: inherit;
    cursor: pointer;
  }

  input.checkbox:after {
    content: "";
    position: absolute;
    top: 4px;
    left: 4px;
    z-index: 1;
    width: 1em;
    height: 1em;
    border: 1px solid #fff;
    -webkit-transition: 0.4s ease-in-out;
    -moz-transition: 0.4s ease-in-out;
    -o-transition: 0.4s ease-in-out;
    transition: 0.4s ease-in-out;
  }

  input.checkbox:checked:after {
    -webkit-transform: rotate(-45deg);
    -moz-transform: rotate(-45deg);
    -o-transform: rotate(-45deg);
    -ms-transform: rotate(-45deg);
    transform: rotate(-45deg);
    height: 0.5rem;
    border-color: #fff;
    border-top-color: transparent;
    border-right-color: transparent;
  }

  .anim input.checkbox:checked:after {
    -webkit-transform: rotate(-45deg);
    -moz-transform: rotate(-45deg);
    -o-transform: rotate(-45deg);
    -ms-transform: rotate(-45deg);
    transform: rotate(-45deg);
    height: 0.5rem;
    border-color: transparent;
    border-right-color: transparent;
    animation: 0.4s rippling 0.4s ease;
    animation-fill-mode: forwards;
  }

  @keyframes rippling {
    50% {
      border-left-color: #fff;
    }
  }
  .clear {
    clear: both;
  }

  body {
    font-family: "Open Sans", sans-serif;
    font-size: 100%;
    background: #2196f3;
  }

  /*--header--*/
  h1 {
    font-size: 3.5em;
    color: #fff;
    letter-spacing: 3px;
    text-align: center;
    margin: 3vw 1vw;
    font-family: "Josefin Sans", sans-serif;
  }

  h1 span,
  h2 i {
    color: #f7296f;
  }

  h2 {
    color: #fff;
    font-size: 22px;
    font-weight: 500;
    letter-spacing: 1px;
    text-transform: capitalize;
    margin-bottom: 1em;
  }

  /*--//header--*/
  /*-- content --*/
  .sub-main-w3 {
    display: -webkit-flex;
    display: -webkit-box;
    display: -moz-flex;
    display: -moz-box;
    display: -ms-flexbox;
    display: flex;
    align-items: center;
    -webkit-box-pack: center;
    -moz-box-pack: center;
    -ms-flex-pack: center;
    -webkit-justify-content: center;
    justify-content: center;
  }

  .sub-main-w3 form {
    max-width: 600px;
    margin: 0 5vw;
    background: rgba(10, 10, 10, 0.17);
    padding: 3.5vw;
    box-sizing: border-box;
    display: -webkit-flex;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    border-bottom: 8px solid #f7296f;
    border-radius: 30px 30px 50px 50px;
  }

  .form-style-agile {
    margin-bottom: 1.5em;
    flex-basis: 100%;
    -webkit-flex-basis: 100%;
  }

  .sub-main-w3 label {
    font-size: 14px;
    color: #fff;
    display: inline-block;
    font-weight: 500;
    margin-bottom: 12px;
    text-transform: capitalize;
    letter-spacing: 1px;
  }

  .sub-main-w3 label i {
    font-size: 16px;
    margin-left: 5px;
    color: #f7296f;
    border-radius: 50%;
    line-height: 1.9;
    text-align: center;
  }

  .form-style-agile input[type="text"],
  .form-style-agile input[type="password"] {
    width: 100%;
    color: #000;
    outline: none;
    font-size: 14px;
    letter-spacing: 1px;
    padding: 15px 15px;
    box-sizing: border-box;
    border: none;
    border: 1px solid #b4b4b4;
    background: #fff;
  }

  .sub-main-w3 input[type="submit"] {
    color: #fff;
    background: #f7296f;
    border: none;
    padding: 13px 0;
    margin-top: 30px;
    outline: none;
    width: 36%;
    font-size: 16px;
    cursor: pointer;
    letter-spacing: 2px;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
    box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.49);
  }

  .sub-main-w3 input[type="submit"]:hover {
    background: #000;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
  }

  .wthree-text {
    width: 100%;
  }

  .wthree-text ul li {
    display: inline-block;
  }

  .wthree-text ul li a {
    color: #fff;
    font-size: 14px;
    letter-spacing: 1px;
    font-weight: 500;
  }

  /*-- checkbox --*/
  .wthree-text label {
    font-size: 15px;
    color: #fff;
    cursor: pointer;
    position: relative;
  }

  .wthree-text {
    text-align: center;
  }

  .anim input.checkbox:checked:after {
    -webkit-transform: rotate(-45deg);
    -moz-transform: rotate(-45deg);
    -o-transform: rotate(-45deg);
    -ms-transform: rotate(-45deg);
    transform: rotate(-45deg);
    height: 0.5rem;
    border-color: transparent;
    border-right-color: transparent;
    animation: 0.4s rippling 0.4s ease;
    animation-fill-mode: forwards;
  }

  @keyframes rippling {
    50% {
      border-left-color: #fff;
    }
    100% {
      border-bottom-color: #fff;
      border-left-color: #fff;
    }
  }

  ::-webkit-input-placeholder {
    color: #000;
  }

  :-moz-placeholder {
    /* Firefox 18- */
    color: #000;
  }

  ::-moz-placeholder {
    /* Firefox 19+ */
    color: #000;
  }

  :-ms-input-placeholder {
    color: #000;
  }

  .footer {
    margin: 4vw 0.3vw 1.6vw;
  }

  .footer p {
    font-size: 14px;
    color: #fff;
    letter-spacing: 2px;
    text-align: center;
    line-height: 1.8;
  }

  .footer p a {
    color: #fff;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
  }

  .footer p a:hover {
    color: #f7296f;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
  }
  .pwd-oper{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
    margin-bottom: 20px;
  }
  .pwd-oper-item:hover{
    cursor:pointer;
  }

  #bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
  }

  #bg canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }
  #bg img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  @media (max-width: 1920px) {
    h1 {
      font-size: 2.8vw;
    }
  }

  @media (max-width: 1024px) {
    h1 {
      font-size: 4.5vw;
    }
  }

  @media (max-width: 800px) {
    h1 {
      font-size: 2.8em;
    }
  }

  @media (max-width: 480px) {
    h1 {
      font-size: 2.5em;
      letter-spacing: 1px;
    }

    .sub-main-w3 form {
      padding: 7.5vw;
    }

    .footer p {
      letter-spacing: 1px;
      font-size: 12px;
      padding: 0 20px;
    }
  }

  @media (max-width: 414px) {
    .form-style-agile input[type="text"],
    .form-style-agile input[type="password"] {
      font-size: 13px;
      padding: 13px 15px;
    }

    .wthree-text ul li:nth-child(1),
    .wthree-text ul li:nth-child(2) {
      float: none;
    }

    .wthree-text ul li:nth-child(2) {
      margin-top: 10px;
    }

    .sub-main-w3 input[type="submit"] {
      width: 56%;
    }

    .wthree-text ul li {
      display: block;
    }
  }

  @media (max-width: 320px) {
    h1 {
      font-size: 2em;
      margin: 5vw 1vw;
    }

    .sub-main-w3 form {
      padding: 25px 14px;
    }
  }

  /*-- //checkbox --*/

  /*--placeholder-color--*/

  ::-webkit-input-placeholder {
    color: #000;
  }

  :-moz-placeholder {
    /* Firefox 18- */
    color: #000;
  }

  ::-moz-placeholder {
    /* Firefox 19+ */
    color: #000;
  }

  :-ms-input-placeholder {
    color: #000;
  }

  /*-- //placeholder-color --*/

  /*-- //content --*/

  /*-- copyright --*/

  .footer {
    margin: 4vw 0.3vw 1.6vw;
  }

  .footer p {
    font-size: 14px;
    color: #fff;
    letter-spacing: 2px;
    text-align: center;
    line-height: 1.8;
  }

  .footer p a {
    color: #fff;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
  }

  .footer p a:hover {
    color: #f7296f;
    -webkit-transition: 0.5s all;
    -o-transition: 0.5s all;
    -moz-transition: 0.5s all;
    -ms-transition: 0.5s all;
    transition: 0.5s all;
  }

  /*-- //copyright --*/

  /*-- bg effect --*/

  #bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
  }

  #bg canvas {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
  }

  /*-- //bg effect --*/

  /*--responsive--*/

  @media (max-width: 1920px) {
    h1 {
      font-size: 2.8vw;
    }
  }

  @media (max-width: 1024px) {
    h1 {
      font-size: 4.5vw;
    }
  }

  @media (max-width: 800px) {
    h1 {
      font-size: 2.8em;
    }
  }

  @media (max-width: 480px) {
    h1 {
      font-size: 2.5em;
      letter-spacing: 1px;
    }

    .sub-main-w3 form {
      padding: 7.5vw;
    }

    .footer p {
      letter-spacing: 1px;
      font-size: 12px;
      padding: 0 20px;
    }
  }

  @media (max-width: 414px) {
    .form-style-agile input[type="text"],
    .form-style-agile input[type="password"] {
      font-size: 13px;
      padding: 13px 15px;
    }

    .wthree-text ul li:nth-child(1),
    .wthree-text ul li:nth-child(2) {
      float: none;
    }

    .wthree-text ul li:nth-child(2) {
      margin-top: 10px;
    }

    .sub-main-w3 input[type="submit"] {
      width: 56%;
    }

    .wthree-text ul li {
      display: block;
    }
  }

  @media (max-width: 320px) {
    h1 {
      font-size: 2em;
      margin: 5vw 1vw;
    }

    .sub-main-w3 form {
      padding: 25px 14px;
    }
  }

  .wxlg {
    margin-left : 30px;
    color: #fff;
    background: #f7296f;
    border: none;
    padding: 13px 0;
    margin-top: 30px;
    outline: none;
    width: 36%;
    font-size: 16px;
    cursor: pointer;
    letter-spacing: 2px;
    box-shadow: 2px 2px 6px rgba(0,0,0,.49);

    -webkit-appearance: push-button;
    user-select: none;
    white-space: pre;
    align-items: flex-start;
    text-align: center;
  }
</style>
