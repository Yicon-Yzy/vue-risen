<template>
  <!--钉钉扫码登录-->
  <div class="ding_wrap" v-show="dingLoginShow">
    <div class="content">
        <div class="close" @click="dingClose">
            <i class="el-icon-close"></i>
        </div>
        <img src="../../assets/img/dl@3x.jpg" class="img">
        <div class="login_wrap" >
          <div  v-if="qrcodeShow">
            <!-- <div class="title">钉钉登录</div> -->
            <div class="" @click="qrcodeClose">
              <i class="el-icon-error title" style="font-size:20px;color:#909399"></i>
            </div>
            <div id="login_container">
            </div>
          </div>
          <div v-else>
            <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login_form">
              <el-form-item prop="userName">
                <el-input v-model="loginForm.userName" type="text" auto-complete="off" placeholder="账号" class="enterpriseName">
                  <svg-icon slot="prefix" icon-class="user" class="el-input__icon input-icon" />
                </el-input>
              </el-form-item>
              <el-form-item prop="password">
                <el-input
                  class="enterpriseName"
                  v-model="loginForm.password"
                  type="password"
                  auto-complete="off"
                  placeholder="密码"
                  @keyup.enter.native="handleLogin"
                >
                  <svg-icon slot="prefix" icon-class="password" class="el-input__icon input-icon" />
                </el-input>
              </el-form-item>
              <!-- <el-form-item prop="captcha"> -->
              <!--   <el-input -->
              <!--     class="enterpriseName" -->
              <!--     v-model="loginForm.captcha" -->
              <!--     auto-complete="off" -->
              <!--     placeholder="验证码" -->
              <!--     style="width: 63%" -->
              <!--     @keyup.enter.native="handleLogin" -->
              <!--   > -->
              <!--     <svg-icon slot="prefix" icon-class="validCode" class="el-input__icon input-icon" /> -->
              <!--   </el-input> -->
              <!--   <div class="login-captcha" v-html="codeHtml" @click="getCode"></div> -->
              <!-- </el-form-item> -->
              <el-checkbox v-model="loginForm.rememberMe" style="margin:0 0 25px 0;">记住密码</el-checkbox>
              <el-form-item style="width:100%;">
                <el-button
                  :loading="loading"
                  size="medium"
                  type="primary"
                  style="width:100%;"
                  @click.native.prevent="handleLogin"
                >
                  <span v-if="!loading">登 录</span>
                  <span v-else>登 录 中...</span>
                </el-button>
              </el-form-item>
            </el-form>
          </div>
        </div>
    </div>
  </div>
</template>

<script>
    export default {
        name: "dingLogin",
        data() {
          return {
            dingLoginShow: false, // 钉钉扫码登录页面
            qrcodeShow:true,
            codeHtml: '',
            cookiePassword: '',
            loginForm: {
              userName: 'admin',
              password: '123456',
              rememberMe: false,
              captcha: '',
              uuid: ''
            },
            loginRules: {
              userName: [
                { required: true, trigger: 'blur', message: '用户名不能为空' }
              ],
              password: [
                { required: true, trigger: 'blur', message: '密码不能为空' }
              ],
              captcha: [{ required: true, trigger: 'change', message: '验证码不能为空' }]
            },
            loading: false,
            redirect: undefined
          }
        },
        methods: {
 //           getCode () {
 //             getCodeImg().then(res => {
 //               this.codeHtml = res
//              })
 //           },
            // 点击登录
            toLogin(jumpurl) {
                // alert(process.env.VUE_APP_REDIRECT_URL)
                this.dingLoginShow = true
                $('.ding_wrap .content').animate({top: '50%'}, 300)
                setTimeout(() => {
                    let url = process.env.VUE_APP_REDIRECT_URL
                    let appid
                    console.log(url)
                    // 正式域名 risenenergy
                    if (url === 'https://guide.risenenergy.com') {
                        appid = 'dingoavjcygk43b1dhxz6u'
                    }
                    // 正式域名 risen
                    else if (url === 'https://guide.risen.com') {
                        // appid = 'dingoa8zpl8uydpwtun5jp'
                        appid = 'dingoavjcygk43b1dhxz6u'
                    }
                    // 测试用
                    else {
                        appid = 'dingoa83fqjqfaigm2ksrr'
                    }
                    url = url + jumpurl
                    console.log(appid)
                    let obj = DDLogin({
                        id: "login_container",//这里需要你在自己的页面定义一个HTML标签并设置id，例如<div id="login_container"></div>或<span id="login_container"></span>
                        goto: encodeURIComponent(`https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid=${appid}&response_type=code&scope=snsapi_login&state=STATE&redirect_uri=${url}`), //请参考注释里的方式
                        style: "border:none;background-color:#FFFFFF;",
                        width: "100%",
                        height: "100%"
                    });
                    var loginTmpCode
                    const _this = this
                    var handleMessage = function (event) {
                        var origin = event.origin;
                        console.log("origin", event.origin);
                        if( origin == "https://login.dingtalk.com" ) { //判断是否来自ddLogin扫码事件。
                            loginTmpCode = event.data;

                            //拿到loginTmpCode后就可以在这里构造跳转链接进行跳转了
                            console.log("loginTmpCode", loginTmpCode);

                            console.log("url",url)
                            url = encodeURIComponent(url)
                            let loadUrl = `https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid=${appid}&response_type=code&scope=snsapi_login&state=STATE&redirect_uri=${url}&loginTmpCode=${loginTmpCode}`
                            if (loginTmpCode)
                              {
                                location.href = loadUrl
                              }
                        }
                    };
                    if (typeof window.addEventListener != 'undefined') {
                        window.addEventListener('message', handleMessage, false);
                    } else if (typeof window.attachEvent != 'undefined') {
                        window.attachEvent('onmessage', handleMessage);
                    }
                }, 300)
            },
            dingClose() {
                $('.ding_wrap .content').animate({top: '-50%'}, 300, () => {
                    this.dingLoginShow = false
                    $('#login_container').html('')
                })
            },
            qrcodeClose() {
              this.qrcodeShow = false;
            },
        },
    }
</script>

<style scoped lang="scss">
    .ding_wrap{
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background-color: rgba(0, 0, 0, .2);
        z-index: 99999;
        backdrop-filter: brightness(150%) saturate(150%) blur(15px);
        .content{
            width: 660px;
            height: 390px;
            position: absolute;
            top: 50%;
            left: 50%;
            background-color: #fff;
            transform: translateX(-50%) translateY(-50%);
            .close{
                position: absolute;
                top: -30px;
                right: 0;
                width: 20px;
                height: 20px;
                line-height: 20px;
                font-size: 22px;
                cursor: pointer;
                i{
                    margin-left: 0;
                    color: #fff;
                }
            }
            .img{
                float: left;
                width: 330px;
                height: 390px;
            }
            .login_wrap{
                float: left;
                width: 330px;
                height: 390px;
                position: relative;
                .title{
                    width: 100%;
                    cursor: pointer;
                    color: #12133A;
                    position: absolute;
                    top: 40px;
                    left: 85%;
                }
                #login_container{
                    width: 330px;
                    height: 320px;
                    margin-top: 25px;
                }
                .login_form{
                  margin:30px;
                  .login-captcha {
                    width: 33%;
                    height: 38px;
                    float: right;
                    box-sizing: border-box;
                    border: 1px solid #bebebe;
                    svg {
                      cursor: pointer;
                      vertical-align: middle;
                      height: 100%;
                      width: 100%;
                    }
                  }
                }
            }
        }
    }

::v-deep .enterpriseName .el-input__inner {
  //background-color: #e8e8e8;
  text-align: center;
  //border-color: #c0c4cc;
  padding:15px 20px;
  //color:#000;
  box-shadow:none;
  border:none;
  border-bottom: thin solid #000000;
  border-radius:unset;
}
</style>
