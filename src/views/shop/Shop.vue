<template>
  <div class="wrapper">
    <div class="search">
      <div class="search__back iconfont" @click="handleBackClick">&#xe6f2;</div>
      <div class="search__content">
        <div class="search__content__icon iconfont">&#xe62d;</div>
        <input class="search__content__input" placeholder="请输入商品名称"/>
      </div>
    </div>
    <ShopInfoVue :item="item" :hideBorder="true"/>
  </div>
</template>
<script>
import { useRouter } from 'vue-router'
import ShopInfoVue from '@/components/ShopInfo.vue'
export default {
  name: 'shop',
  components: { ShopInfoVue },
  setup () {
    const router = useRouter()
    const item = {
      _id: '1',
      name: '沃尔玛',
      sales: 10000,
      expressLimit: 0,
      expressPrice: 5,
      slogan: 'VIP尊享八折优惠'
    }
    const handleBackClick = () => {
      router.back()
    }
    return { item, handleBackClick }
  }
}
</script>
<style lang='scss' scoped>
.wrapper{
  padding: 0 .18rem;
}
.search{
  display: flex;
  margin: .2rem 0 .16rem 0;
  line-height: .32rem;
  &__back{
    width: .3rem;
    font-size: .24rem;
    color: #b6b6b6;
  }
  &__content{
    display: flex;
    flex: 1;
    background: #f5f5f5;
    border-radius: .16rem;
    &__icon{
      width: .44rem;
      text-align: center;
      color: #b7b7b7;
    }
    &__input{
      display: block;
      width: 100%;
      height: .32rem;
      padding-right: .2rem;
      font-size: .14rem;
      border: none;
      outline: none;
      background: none;
      color: #333;
      ::placeholder{
        color: #333;
      }
    }
  }
}
</style>
