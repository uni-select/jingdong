<template>
  <div class="wrapper">
    <ShopInfoVue :item="item" :hideBorder="true"/>
  </div>
</template>
<script>
import ShopInfoVue from '@/components/ShopInfo.vue'
export default {
  name: 'shop',
  components: { ShopInfoVue },
  setup () {
    const item = {
      _id: '1',
      name: '沃尔玛',
      sales: 10000,
      expressLimit: 0,
      expressPrice: 5,
      slogan: 'VIP尊享八折优惠'
    }
    return { item }
  }
}
</script>
<style lang='scss' scoped>
.wrapper{
  padding: 0 .18rem;
}
</style>
