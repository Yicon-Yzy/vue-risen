<template>
  <!--钉钉扫码登录-->
  <div class="ding_wrap" v-show="dingLoginShow">
    <div class="content">
        <div class="close" @click="dingClose">
            <i class="el-icon-close"></i>
        </div>
        <img src="../../assets/img/dl@3x.jpg" class="img">
        <div class="login_wrap">
            <div class="title">钉钉登录</div>
            <div id="login_container"></div>
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
          }
        },
        methods: {
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
                    height: 25px;
                    line-height: 25px;
                    text-align: center;
                    font-size: 18px;
                    color: #12133A;
                    position: absolute;
                    top: 40px;
                    left: 0;
                }
                #login_container{
                    width: 330px;
                    height: 320px;
                    margin-top: 50px;
                }
            }
        }
    }
</style>
