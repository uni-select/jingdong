<template>
    <div class="wrapper">
      <img class="wrapper__img" src="http://www.dell-lee.com/imgs/vue3/user.png" >
      <div class="wrapper__input">
          <input class="wrapper__input__content" v-model="username" placeholder="请输入用户名">
      </div>
      <div class="wrapper__input">
          <input class="wrapper__input__content" v-model="password" placeholder="请输入密码" type="password">
      </div>
      <div class="wrapper__login-button" @click="handleLogin">登陆</div>
      <div class="wrapper__login-link" @click="handleRegisterClick">立即注册</div>
      <Toast v-if="show" :message="toastMessage" />
    </div>
</template>
<script>
import { reactive, toRefs } from 'vue'
import { useRouter } from 'vue-router'
import { post } from '../../utils/request'
import Toast, { useToastEffect } from '../../components/Toast'
// 登陆流程逻辑
const useLoginEffect = (showToast) => {
  const router = useRouter()
  const data = reactive({ username: '', password: '' })
  const handleLogin = async () => {
    try {
      const { username, password } = data
      if (!username || !password) {
        showToast('用户名或密码不能为空')
        return
      }
      const result = await post('/api/user/login', {
        username: data.username,
        password: data.password
      })
      console.log('登陆请求返回数据', result)
      if (result?.errno === 0) {
        localStorage.isLogin = true
        router.push({ name: 'Home' })
      } else {
        showToast('登陆失败')
      }
    } catch (error) {
      showToast('请求失败')
    }
  }
  const { username, password } = toRefs(data)
  return { username, password, handleLogin }
}
// 注册流程逻辑
const useRegisterEffect = () => {
  const router = useRouter()
  const handleRegisterClick = () => {
    router.push({ name: 'Register' })
  }
  return { handleRegisterClick }
}
export default {
  name: 'Login',
  components: { Toast },
  // setup职责，告诉你代码执行的流程
  setup () {
    const { showToast, show, toastMessage } = useToastEffect()
    const { handleLogin, username, password } = useLoginEffect(showToast)
    const { handleRegisterClick } = useRegisterEffect(showToast)
    return { show, toastMessage, username, password, handleLogin, handleRegisterClick }
  }
}
</script>
<style lang='scss' scoped>
@import '../../style/viriables.scss';
.wrapper {
    position: absolute;
    top: 50%;
    left: 0;
    right: 0;
    transform: translateY(-50%);
    &__img {
        display: block;
        width: .66rem;
        height: .66rem;
        margin: 0 auto .4rem auto;
    }
    &__input {
        padding: 0 .16rem;
        height: .48rem;
        margin: 0 .4rem .16rem .4rem;
        background: #F9F9F9;
        border: 1px solid rgba(0,0,0,.1);
        border-radius: .06rem;
        &__content{
            width: 100%;
            border: none;
            outline: none;
            line-height: .48rem;
            background: none;
            font-size: .16rem;
            // color: $content-notice-fontcolor;
            &::placeholder{
                color: $content-notice-fontcolor;
            }
        }
    }
    &__login-button{
        background: #0091FF;
        box-shadow: 0 .04rem .08rem 0 rgba(0,145,255,.32);
        border-radius: .04rem;
        color: #fff;
        margin: .32rem .4rem .16rem .4rem;
        line-height: .48rem;
        font-size: .16rem;
        text-align: center;
    }
    &__login-link{
        font-size: .14rem;
        text-align: center;
        color: $content-notice-fontcolor;
    }
}
</style>
