<template>
    <div class="WhiteScroll home_wrap">
        <img class="title" src="@/assets/img/hy@3x.jpg">
        <img class="title_2" src="@/assets/img/zy@3x.jpg">
        <div class="grid">
            <img src="@/assets/img/rz@3x.jpg" class="one" @click="openFile('rz')">
            <img src="@/assets/img/wp@3x.jpg" class="one" @click="openFile('wp')">
            <img src="@/assets/img/yx@3x.jpg" class="one" @click="openFile('yx')">
            <img src="@/assets/img/dyj@3x.jpg" class="one" @click="openFile('dyj')">
            <img src="@/assets/img/wx@3x.jpg" class="one" @click="openFile('wx')">
            <img src="@/assets/img/oa@3x.jpg" class="one" @click="openFile('oa')">
            <img src="@/assets/img/yzm@3x.jpg" class="one" @click="openFile('yzm')">
            <img src="@/assets/img/vpn@3x.jpg" class="one" @click="openFile('vpn')">
            <img src="@/assets/img/gszl@3x.jpg" class="one" @click="openFile('gszl')">
            <img src="@/assets/img/gd@3x.jpg" class="one" @click="openFile('gd')">
        </div>
        <div style="clear: both;"></div>
        <div class="phone">反馈电话:13857879705</div>
        <div class="bottom"></div>
        <footer class="footer">
            <div class="copyright">
                  <span class="copyright-content">
                      Copyright ©
                      <a href="http://www.risenenergy.com/" target="_blank">东方日升新能源股份有限公司 </a>
                      All Rights Reserved.
                      <a href="http://beian.miit.gov.cn/" target="_blank">浙ICP备15027266号</a>
                      <i class="btn icon-information-solid p-5 copyright-open"></i>
                  </span>
            </div>
        </footer>
    </div>
</template>

<script>
    export default {
        name: "home",
        methods: {
            // 打开引导文档
            openFile(flag) {
                // 入职
                if (flag === 'rz') {
                    window.open('https://mp.weixin.qq.com/s/eoypPeXcfw1xHfyX3jLjRQ')
                }
                // 网盘
                else if (flag === 'wp') {
                    this.$router.push({
                        path: '/guide',
                        query: {
                            name: 'pan'
                        }
                    })
                    // window.open('http://guide.risenenergy.com/src/pan.pdf')
                }
                // 邮箱
                else if (flag === 'yx') {
                    this.$router.push({
                        path: '/guide',
                        query: {
                            name: 'mail'
                        }
                    })
                    // window.open('http://guide.risenenergy.com/src/mail.pdf')
                }
                // 打印机
                else if (flag === 'dyj') {
                    this.$router.push({
                        path: '/guide',
                        query: {
                            name: 'dyj'
                        }
                    })
                    // window.open('http://guide.risenenergy.com/src/dyj.pdf')
                }
                // wifi
                else if (flag === 'wx') {
                    this.$router.push({
                        path: '/guide',
                        query: {
                            name: 'wifi'
                        }
                    })
                    // window.open('http://guide.risenenergy.com/src/wifi.pdf')
                }
                // oa
                else if (flag === 'oa') {
                    // window.open('http://guide.risenenergy.com/src/oa.pdf')
                    window.open('https://oa.risenenergy.com:333/')
                }
                // 云桌面
                else if (flag === 'yzm') {
                    this.$router.push({
                        path: '/guide',
                        query: {
                            name: 'yzm'
                        }
                    })
                    // window.open('http://guide.risenenergy.com/src/yzm.pdf')
                }
                // vpn
                else if (flag === 'vpn') {
                    this.$router.push({
                        path: '/guide',
                        query: {
                            name: 'vpn'
                        }
                    })
                    // window.open('http://guide.risenenergy.com/src/yzm.pdf')
                }
                // 公司资料
                else if (flag === 'gszl') {
                    window.open('https://guide.risen.com/src/gszl.html')
                }
                // more
                else if (flag === 'gd') {}
            },
        },
    }
</script>

<style scoped lang="scss">
    .home_wrap{
        background-color: #fff;
        margin-top: 30px;
        height: calc(100vh - 111px);
        overflow-y: auto;
    }
    .title{
        margin: 50px auto 0;
        display: block;
        width: 100%;
        max-width: 540px;
    }
    .title_2{
        margin: 100px auto 0;
        display: block;
        width: 100%;
        max-width: 286px;
    }
    .line{
        /*display: flex;*/
        margin-top: 60px;
        .one{
        }
        .between{
            /*flex: 1;*/
        }
    }
    .grid{
        width: 66%;
        margin: 40px auto 0;
        .one{
            float: left;
            margin: 0 5% 50px;
            width: 15%;
            cursor: pointer;
        }
    }
    .bottom{
        height: 100px;
    }
    .phone{
        margin-top: 20px;
        text-align: center;
        color: #666;
        display: none;
    }
</style>
