<template>
    <div class="wrapper">
        <img class="wrapper__img" src="http://www.dell-lee.com/imgs/vue3/user.png" >
        <div class="wrapper__input">
            <input class="wrapper__input__content" v-model="username" placeholder="请输入用户名">
        </div>
        <div class="wrapper__input">
            <input class="wrapper__input__content" v-model="password" placeholder="请输入密码" type="password">
        </div>
        <div class="wrapper__input">
            <input class="wrapper__input__content" v-model="ensurement" placeholder="请确认密码" type="password">
        </div>
        <div class="wrapper__register-button" @click="handleRegister">注册</div>
        <div class="wrapper__register-link" @click="handleLoginClick">已有账号去登陆</div>
        <Toast v-if="show" :message="toastMessage" />
    </div>
</template>
<script>
import { reactive, toRefs } from 'vue'
import { useRouter } from 'vue-router'
import { post } from '../../utils/request'
import Toast, { useToastEffect } from '../../components/Toast'
// 处理注册逻辑
const useRegisterEffect = (showToast) => {
  const router = useRouter()
  const data = reactive({ username: '', password: '', ensurement: '' })
  //   注册按钮点击事件
  const handleRegister = async () => {
    try {
      const result = await post('/api/user/register', {
        username: data.username,
        password: data.password
      })
      console.log('注册请求返回数据', result)
      if (result?.errno === 0) {
        router.push({ name: 'Login' })
      } else {
        showToast('注册失败')
      }
    } catch (error) {
      showToast('请求失败')
    }
  }
  const { username, password, ensurement } = toRefs(data)
  return { username, password, ensurement, handleRegister }
}
export default {
  name: 'Register',
  components: { Toast },
  setup () {
    const router = useRouter()
    const { showToast, show, toastMessage } = useToastEffect()
    const { username, password, ensurement, handleRegister } = useRegisterEffect(showToast)
    // 跳回登陆页面
    const handleLoginClick = () => {
      router.push({ name: 'Login' })
    }
    return { show, toastMessage, username, password, ensurement, handleRegister, handleLoginClick }
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
    &__register-button{
        background: #0091FF;
        box-shadow: 0 .04rem .08rem 0 rgba(0,145,255,.32);
        border-radius: .04rem;
        color: #fff;
        margin: .32rem .4rem .16rem .4rem;
        line-height: .48rem;
        font-size: .16rem;
        text-align: center;
    }
    &__register-link{
        font-size: .14rem;
        text-align: center;
        color: $content-notice-fontcolor;
    }
}
</style>
