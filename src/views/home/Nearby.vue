<template>
    <div class="nearby">
      <h3 class="nearby__title">附近店铺</h3>
      <router-link to="/shop" v-for="item in nerabyList" :key="item._id">
        <ShopInfo :item="item" />
      </router-link>
    </div>
</template>
<script>
import { ref } from 'vue'
import { get } from '../../utils/request'
import ShopInfo from '@/components/ShopInfo.vue'
const useNerabyListEffect = () => {
  const nerabyList = ref([])
  const getNearList = async () => {
    const result = await get('/api/shop/hot-list')
    if (result?.errno === 0 && result?.data?.length) {
      nerabyList.value = result.data
    }
    console.log('热门列表查询返回结果', result)
  }
  return { nerabyList, getNearList }
}
export default {
  name: 'Nearby',
  components: { ShopInfo },
  setup () {
    const { nerabyList, getNearList } = useNerabyListEffect()
    getNearList()
    return { nerabyList }
  }
}
</script>
<style lang='scss' scoped>
@import '../../style/viriables.scss';
.nearby{
  &__title{
    margin: .16rem 0 .02rem 0;
    font-size: .18rem;
    font-weight: normal;
    color: $content-fontcolor;
  }
  a{
    text-decoration: none;
  }
}
</style>
