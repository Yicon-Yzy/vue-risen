<template>
  <div class="login" v-loading = "loading">
    111

  </div>
</template>

<script>
import { getCodeImg, dinglogin} from '@/api/admin/login'
import Cookies from 'js-cookie'
import { encrypt, decrypt } from '@/utils/jsencrypt'

export default {
  name: 'DingLogin',
  data () {
    return {
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
  async created () {
    const { params, query } = this.$route
    let  redirect = query.redirect
    //第一次分裂取最后一个，第二次分裂取最后一个
    let  code = redirect.split('?')
    redirect = code [0]
    if (code.length === 2){
      code = code[1].split('=')[1]
      this.loading = true
      this.$store
        .dispatch('DingLogin', {code:code})
        .then(() => {
          this.loading = false
          this.$router.push({
            path: redirect
          })
        })
        .catch(() => {
          this.loading = false
          this.getCode()
        })
    }
    this.getCookie()
  },
  methods: {
    getCookie () {
      const userName = Cookies.get('userName')
      const password = Cookies.get('password')
      const rememberMe = Cookies.get('rememberMe')
      this.loginForm = {
        userName: userName === undefined ? this.loginForm.userName : userName,
        password: password === undefined ? this.loginForm.password : decrypt(password),
        rememberMe: rememberMe === undefined ? false : Boolean(rememberMe)
      }
    },
  }
}
</script>

<style rel="stylesheet/scss" lang="scss">
</style>
