<template>
  <div>
    <div id="bg" style=" background:#2196f3">
      <canvas></canvas>
      <canvas></canvas>
      <canvas></canvas>
    </div>
    <h1 class="visible-lg-block visible-md-block">{{sysname}}</h1>
    <div class="sub-main-w3">
      <form action="#" @submit.prevent="loginNow">
        <h2>用户登录
          <i class="iconfont icon-jiantouarrow511"></i>
        </h2>
        <div class="form-style-agile">
          <label>
            <i class="iconfont icon-yonghu"></i>
            用户名
          </label>
          <input placeholder="请输入用户名" maxlength="32" v-model="login.username" type="text" required
                 oninvalid="setCustomValidity('请输入用户名')" oninput="setCustomValidity('')">
        </div>
        <div class="form-style-agile">
          <label>
            <i class="iconfont icon-xiugaimima"></i>
            密码
          </label>
          <input placeholder="请输入密码" maxlength="32" v-model="login.password" type="password" required
                 oninvalid="setCustomValidity('请输入密码')" oninput="setCustomValidity('')">
        </div>
        <!-- checkbox -->
        <input type="submit" class="submit" value="立即登录">
      </form>
    </div>
    <div class="footer">
      <p>{{copyright}}</p>
    </div>

  </div>
</template>

<script>
  import {drawDynamicsBG} from "../../assets/js/bg";
  import axios from 'axios'

  export default {
    name: "loginWithUp",
    props: {
      host: String,         //登录地址
      sysname: String,      //系统名称
      copyright: String,    //版权信息
      ident: String         //系统英文名称
    },
    data() {
      return {
        login: {
          username: "",
          password: "",
          ident: this.ident
        },
      };
    },
    mounted() {
      sessionStorage.removeItem("__jwt__");
      sessionStorage.removeItem("code");
      drawDynamicsBG("bg");
    },
    methods: {
      loginNow() {
        self = this;
        axios.post(this.host, this.login)
          .then(function (response) {

            sessionStorage.setItem("username", self.login.username);
            if (!response.headers.__jwt__) {
              self.$emit('loginCall', false);
            }
            sessionStorage.setItem("__jwt__", response.headers.__jwt__);
            if (!response.data.code) {
              self.$emit('loginCall', false);
            }
            sessionStorage.setItem("code", response.data.code);

            self.$emit('loginCall', true);
          })
          .catch(function (error) {
            self.$emit('loginCall', false);
          });
      }
    }
  };
</script>

<style scoped>

  @font-face {
    font-family: "iconfont";
    src: url('//at.alicdn.com/t/font_889924_0j59ab18fkol.eot?t=1540460691538'); /* IE9*/
    src: url('//at.alicdn.com/t/font_889924_0j59ab18fkol.eot?t=1540460691538#iefix') format('embedded-opentype'), /* IE6-IE8 */ url('data:application/x-font-woff;charset=utf-8;base64,d09GRgABAAAAAAVcAAsAAAAACAgAAQAAAAAAAAAAAAAAAAAAAAAAAAAAAABHU1VCAAABCAAAADMAAABCsP6z7U9TLzIAAAE8AAAARAAAAFY8qUirY21hcAAAAYAAAABfAAABnLRmHZdnbHlmAAAB4AAAAWkAAAG8CygXK2hlYWQAAANMAAAALwAAADYTDqchaGhlYQAAA3wAAAAcAAAAJAfeA4VobXR4AAADmAAAAA4AAAAQEAAAAGxvY2EAAAOoAAAACgAAAAoBKgCWbWF4cAAAA7QAAAAeAAAAIAESADxuYW1lAAAD1AAAAUUAAAJtPlT+fXBvc3QAAAUcAAAAPgAAAE+eW8MOeJxjYGRgYOBikGPQYWB0cfMJYeBgYGGAAJAMY05meiJQDMoDyrGAaQ4gZoOIAgCKIwNPAHicY2BkYWCcwMDKwMHUyXSGgYGhH0IzvmYwYuRgYGBiYGVmwAoC0lxTGByeGT/bydzwv4EhhrmBoQEozAiSAwDvDwzPeJztkLENgDAMBM9JQAgxCh0IKetQsSILZY3gfCgYgrfOsl+WiwcGIDqrk8AujKbTXZMfmeUnDt8nrwBlK7nctX4nyXQx6WtoH2zk16K+v1tsqXU0545yvTvYA225Fb4AeJxNkN9KAlEQxuc7YyubsiWrlhcVuqbQkp4t2/WiP3QRCEFRoBd50fYWirkhQUFP0AOYFz1DRQ8SPURgdKN2NoPiHL5vhvlgfgwJoskHB2zQCpWJkLJyUQNL8Nz0HLRcGYWK565gD1bB9VzPzBqIppaxkN3FDioFDt5P7CWzG9VnnniWn4SYiV4C4Mz20fXp6EGPATFdNPV4nI2WnavNNxyOa8NIZMiaes7enH57cIzRvYrdTdPKiCKK65lveJ/ilKE1shWbWqu2l1Dchef8b3hTdSkDRUsrFLfCKT4vekL0LvxQ/f91/aU7YB50f433OTj3A+bAPw/1rx4Nud8x19NCJMtmp89E6k9euaGYFimrmAjWlgIIL2IpSUzP4iVKQAkVdyOd1MxsiKMGTLZoNZstYeclIPOCfnz8lpeyJqU//sJhtXoIka6ftYVon9UhV8eNaRiPq3Lcg6w5Tk3iqq5yKv0NQpNWNwAAAHicY2BkYGAA4g8qz9/H89t8ZeBmYQCB6983TkbQ//eyMDA7ArkcDEwgUQB6vwzNAHicY2BkYGBu+N/AEMPCAAJAkpEBFbAAAEcKAm14nGNhYGBgQcIAALAAEQAAAAAAAABMAJYA3gAAeJxjYGRgYGBhMABiEGACYi4gZGD4D+YzAAANwgFPAAB4nGWPTU7DMBCFX/oHpBKqqGCH5AViASj9EatuWFRq911036ZOmyqJI8et1ANwHo7ACTgC3IA78EgnmzaWx9+8eWNPANzgBx6O3y33kT1cMjtyDRe4F65TfxBukF+Em2jjVbhF/U3YxzOmwm10YXmD17hi9oR3YQ8dfAjXcI1P4Tr1L+EG+Vu4iTv8CrfQ8erCPuZeV7iNRy/2x1YvnF6p5UHFockikzm/gple75KFrdLqnGtbxCZTg6BfSVOdaVvdU+zXQ+ciFVmTqgmrOkmMyq3Z6tAFG+fyUa8XiR6EJuVYY/62xgKOcQWFJQ6MMUIYZIjK6Og7VWb0r7FDwl57Vj3N53RbFNT/c4UBAvTPXFO6stJ5Ok+BPV8bUnV0K27LnpQ0kV7NSRKyQl7WtlRC6gE2ZVeOEXpc0Yk/KGdI/wAJWm7IAAAAeJxjYGKAAC4G7ICFkYmRmZGFkZWBPyszMa8kvzSxqCi/3NTQkK0yPy89o5SrIrM0PTEzNzM3kYEBAPk2DMMAAA==') format('woff'),
    url('//at.alicdn.com/t/font_889924_0j59ab18fkol.ttf?t=1540460691538') format('truetype'), /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/ url('//at.alicdn.com/t/font_889924_0j59ab18fkol.svg?t=1540460691538#iconfont') format('svg'); /* iOS 4.1- */
  }

  .iconfont {
    font-family: "iconfont" !important;
    font-size: 20px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  .icon-jiantouarrow511:before {
    content: "\e6b9";
  }

  .icon-yonghu:before {
    content: "\e639";
  }

  .icon-xiugaimima:before {
    content: "\e633";
  }

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
    border: 1px solid #000;
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
    float: right;
  }

  .wthree-text ul li:nth-child(2) {
    float: left;
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
    border: 1px solid #000;
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
    margin: 4vw 0.3vw 2vw;
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
    margin: 4vw 0.3vw 2vw;
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

  /*--//responsive--*/
</style>
