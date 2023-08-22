<template>
  <div class="wrapper">
    <div class="search">
      <div class="search__back iconfont" @click="handleBackClick">&#xe6f2;</div>
      <div class="search__content">
        <div class="search__content__icon iconfont">&#xe62d;</div>
        <input class="search__content__input" placeholder="请输入商品名称"/>
      </div>
    </div>
    <ShopInfoVue :item="item" :hideBorder="true" v-show="item.imgUrl"/>
    <Content />
  </div>
</template>
<script>
import { reactive, toRefs } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import { get } from '@/utils/request'
import ShopInfoVue from '@/components/ShopInfo'
import Content from './Content'
// 商品详情页请求数据
const useShopInfoEffect = () => {
  const route = useRoute()
  const data = reactive({ item: {} })
  const getItemData = async () => {
    const result = await get(`/api/shop/${route.params.id}`)
    if (result?.errno === 0 && result?.data) {
      data.item = result.data
    }
  }
  const { item } = toRefs(data)
  return { item, getItemData }
}
const useBackRouterEffect = () => {
  const router = useRouter()
  const handleBackClick = () => {
    router.back()
  }
  return { handleBackClick }
}
export default {
  name: 'shop',
  components: { ShopInfoVue, Content },
  setup () {
    const { item, getItemData } = useShopInfoEffect()
    const { handleBackClick } = useBackRouterEffect()
    getItemData()
    return { item, handleBackClick }
  }
}
</script>
<style lang='scss' scoped>
@import '../../style/viriables.scss';
.wrapper{
  padding: 0 .18rem;
}
.search{
  display: flex;
  margin: .14rem 0 .04rem 0;
  line-height: .32rem;
  &__back{
    width: .3rem;
    font-size: .24rem;
    color: #b6b6b6;
  }
  &__content{
    display: flex;
    flex: 1;
    background: $search-bgColor;
    border-radius: .16rem;
    &__icon{
      width: .44rem;
      text-align: center;
      color: $search-fontColor;
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
      color: $content-fontcolor;
      ::placeholder{
        color: $content-fontcolor;
      }
    }
  }
}
</style>
