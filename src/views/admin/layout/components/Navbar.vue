<template>
  <div class="navbar">
    <dingtalk-login ref="login"></dingtalk-login>
    <h3 class="title">后台管理系统</h3>

    <div class="right-menu">
      <template v-if="device!=='mobile'">
        <el-tooltip content="刷新" effect="dark" placement="bottom">
          <div class="right-menu-item" @click="reload">
            <svg-icon icon-class="refresh" />
          </div>
        </el-tooltip>

        <el-tooltip content="全屏" effect="dark" placement="bottom">
          <div class="right-menu-item">
            <screenfull id="screenfull" @click="reload" class="hover-effect" />
          </div>
        </el-tooltip>

        <el-tooltip content="布局大小" effect="dark" placement="bottom">
          <size-select id="size-select" class="right-menu-item hover-effect" />
        </el-tooltip>
        <el-tooltip :content="houseContent"  effect="dark" placement="bottom" >
          <div class="right-menu-item" @click ="flip">
            <svg-icon icon-class="nav-home" />
          </div>
        </el-tooltip>

      </template>

      <el-dropdown class="avatar-container right-menu-item hover-effect" trigger="click">
        <div class="avatar-wrapper">
          <img :src="avatar" class="user-avatar">
          <i class="el-icon-caret-bottom" />
        </div>
        <el-dropdown-menu slot="dropdown">
          <router-link to="/user/profile">
            <el-dropdown-item>个人中心</el-dropdown-item>
          </router-link>
          <el-dropdown-item @click.native="setting = true">
            <span>布局设置</span>
          </el-dropdown-item>
          <router-link to="/article/blogManage">
            <el-dropdown-item>
              博客管理
            </el-dropdown-item>
          </router-link>
          <el-dropdown-item divided @click.native="logout">
            <span>退出登录</span>
          </el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
    </div>
  </div>
</template>

<script>
import { getToken } from '@/utils/auth'
import { mapGetters } from 'vuex'
import Screenfull from '@/components/Screenfull/index'
import DingtalkLogin from '@/components/DingtalkLogin/index'
import SizeSelect from '@/components/SizeSelect/index'
import { baseImgUrl } from '@/utils/config'

export default {
  inject: ['reload'],
  components: {
    Screenfull,
    DingtalkLogin,
    SizeSelect
  },
  data () {
    return {
      avatar: '',
      whichtype:'',
    }
  },
  computed: {
    ...mapGetters([
      'sidebar',
      'device'
    ]),
    houseContent(){
      const whichtype = this.whichtype
      return whichtype == 'front' ? '后台管理':'导航系统'
    },
    setting: {
      get () {
        return this.$store.state.settings.showSettings
      },
      set (val) {
        this.$store.dispatch('settings/changeSetting', {
          key: 'showSettings',
          value: val
        })
      }
    }
  },
  created () {
    const fullPath = this.$router.history.current.fullPath
    this.whichtype = 'admin'
    if (fullPath.match('front')){
      this.whichtype = 'front'
    }
    // 先加载默认头像。如果有自定义头像就替换
    this.avatar = require('@/static/images/profile.jpg')
    if (this.$store.getters.userInfo) {
      const avatar = this.$store.getters.userInfo.user.avatar;
      if (avatar) {
        if (avatar.match("dingtalk")) // 从钉钉获取的头像图片
        {
          this.avatar = avatar;
        }
        else //后台注册的用户头像图片
        {
          this.avatar = baseImgUrl + this.$store.getters.userInfo.user.avatar
        }
      }
    }
  },
  mounted() {
  },
  methods: {
    toggleSideBar () {
      this.$store.dispatch('app/toggleSideBar')
    },
    flip() {
      if (this.whichtype === 'front'){
        //TODO
        //前面需要加一个登录功能
        //如果是有token则需要直接跳转
        //否则增加popup a window to login
        if (getToken()) {
          this.$router.replace('/layout/home')
        }else{
          this.$refs.login.toLogin()
        }
      }else{
        this.$router.replace('/front/navigator/home')
      }
    },
    async logout () {
      this.$confirm('确定注销并退出系统吗？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.$store.dispatch('LogOut').then(() => {
          location.reload()
        })
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.navbar {
  height: 50px;
  overflow: hidden;
  position: relative;
  background: #fff;
  box-shadow: 0 1px 4px rgba(0,21,41,.08);

  .title{
    display: inline-block;
    line-height: 50px;
    height: 50px;
    margin: 0 0 0 15px;
  }

  .hamburger-container {
    line-height: 46px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background .3s;
    -webkit-tap-highlight-color:transparent;

    &:hover {
      background: rgba(0, 0, 0, .025)
    }
  }

  .breadcrumb-container {
    float: left;
  }

  .errLog-container {
    display: inline-block;
    vertical-align: top;
  }

  .right-menu {
    float: right;
    height: 100%;
    line-height: 50px;

    &:focus {
      outline: none;
    }

    .right-menu-item {
      display: inline-block;
      padding: 0 8px;
      height: 100%;
      font-size: 18px;
      color: #5a5e66;
      vertical-align: text-bottom;
      cursor: pointer;

      &.hover-effect {
        cursor: pointer;
        transition: background .3s;

        &:hover {
          background: rgba(0, 0, 0, .025)
        }
      }
    }

    .avatar-container {
      margin-right: 30px;

      .avatar-wrapper {
        margin-top: 5px;
        position: relative;

        .user-avatar {
          cursor: pointer;
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }

        .el-icon-caret-bottom {
          cursor: pointer;
          position: absolute;
          right: -20px;
          top: 25px;
          font-size: 12px;
        }
      }
    }
  }
}
</style>
