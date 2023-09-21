<template>
  <div class="chat">
    <div class="product">
      <div class="product__header">
        <div class="product__header__all">
          <span class="product__header__icon iconfont">&#xe6f7;</span>
          全选
        </div>
        <div class="product__header__clear" @click="() => cleanCartProducts(shopId)">清空购物车</div>
      </div>
      <template
        v-for="item in productList"
        :key="item._id">
        <div v-if="item.count > 0" class="product__item">
          <div
            v-html="item.check ? '&#xe652;' : '&#xe6f7;'"
            @click="() => chengeCartItemChecked(shopId, item._id)"
            class="iconfont product__item__checked">
          </div>
          <img class="product__item__img" :src="item.imgUrl" />
          <div class="product__item__detail">
            <h4 class="product__item__title">{{item.name}}</h4>
            <p class="product__item__price">
              <span class="product__item__yen">&yen;</span>{{item.price}}
              <span class="product__item__origin">&yen;{{item.oldPrice}}</span>
            </p>
          </div>
          <div class="product__number">
            <span class="product__number__minus" @click="changeCartItemInfo(shopId, item._id, item, -1)">-</span>
            {{ item.count || 0 }}
            <span class="product__number__plus" @click="changeCartItemInfo(shopId, item._id, item, 1)">+</span>
          </div>
        </div>
      </template>
    </div>
    <div class="check">
      <div class="check__icon">
        <img src="http://www.dell-lee.com/imgs/vue3/basket.png" class="check__icon__img">
        <div class="check__icon__tag">{{total}}</div>
      </div>
      <div class="check__info">
        总计：<span class="check__info__price">&yen; {{price}}</span>
      </div>
      <div class="check__btn">去结算</div>
    </div>
  </div>
</template>
<script>
import { computed } from 'vue'
import { useStore } from 'vuex'
import { useRoute } from 'vue-router'
import { useCommonCartEffect } from './commonChatEffect'
// 获取购物车相关逻辑
const useCartEffect = (shopId) => {
  const { changeCartItemInfo } = useCommonCartEffect()
  const store = useStore()
  const cartList = store.state.cartList
  const total = computed(() => {
    const productList = cartList[shopId]
    let count = 0
    if (productList) {
      for (const i in productList) {
        const product = productList[i]
        count += product.count
      }
    }
    return count
  })
  const price = computed(() => {
    const productList = cartList[shopId]
    let count = 0
    if (productList) {
      for (const i in productList) {
        const product = productList[i]
        if (product.check) {
          count += (product.count * product.price)
        }
      }
    }
    return count.toFixed(2)
  })
  const productList = computed(() => {
    const productList = cartList[shopId] || []
    return productList
  })
  const chengeCartItemChecked = (shopId, productId) => {
    store.commit('changeCartItemChecked', { shopId, productId })
  }
  const cleanCartProducts = (shopId) => {
    store.commit('cleanCartProducts', { shopId })
  }
  return { total, price, productList, changeCartItemInfo, chengeCartItemChecked, cleanCartProducts }
}
export default {
  name: 'Chat',
  setup () {
    const route = useRoute()
    const shopId = route.params.id
    const { total, price, productList, changeCartItemInfo, chengeCartItemChecked, cleanCartProducts } = useCartEffect(shopId)
    return { total, price, shopId, productList, changeCartItemInfo, chengeCartItemChecked, cleanCartProducts }
  }
}
</script>
<style lang='scss' scoped>
@import '@/style/mixins.scss';
.chat{
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
}
.product{
  flex: 1;
  overflow-y: scroll;
  background: #fff;
  &__header{
    height: .52rem;
    line-height: .52rem;
    border-bottom: 1px solid #F1F1F1;
    display: flex;
    font-size: .14rem;
    color: #333;
    &__all{
      width: .64rem;
      margin-left: .18rem;
      &__icon{
        font-size: .2rem;
        color:#0091ff;
      }
    }
    &__clear{
      flex: 1;
      margin-right: .18rem;
      text-align: right;
    }
  }
  &__item{
    position: relative;
    display: flex;
    padding: .12rem 0;
    margin: 0 .16rem;
    border-bottom: .01rem solid #F1F1F1;
    &__checked{
      line-height: .5rem;
      margin-right: .2rem;
      font-size: .2rem;
      color: #0091ff;
    }
    &__img{
      width: .46rem;
      height: .46rem;
      margin-right: .16rem;
    }
    &__detail{
      overflow: hidden;
    }
    &__title{
      margin: .06rem 0 0 0;
      line-height: .2rem;
      font-size: .14rem;
      color: #333;
      @include ellipsis;
    }
    &__price{
      margin: 0;
      line-height: .2rem;
      font-size: .14rem;
      color: #e93b3b;
    }
    &__yen{
      font-size: .12rem;
    }
    &__origin{
      line-height: .2rem;
      font-size: .12rem;
      color: #999;
      text-decoration: line-through;
    }
    .product__number{
      position: absolute;
      bottom: .12rem;
      right: 0;
      &__minus, &__plus{
        display: inline-block;
        width: .2rem;
        height: .2rem;
        line-height: .16rem;
        border-radius: 50%;
        font-size: .2rem;
        text-align: center;
      }
      &__minus{
        border: 1px solid #666;
        color: #666;
        margin-right: .05rem;
      }
      &__plus{
        background: #0091ff;
        color: #fff;
        margin-left: .05rem;
      }
    }
  }
}
.check{
  display: flex;
  height: .49rem;
  line-height: .49rem;
  border-top: .01rem solid #f1f1f1;
  &__icon{
    position: relative;
    width: .84rem;
    &__img{
      display: block;
      margin: .12rem auto;
      width: .28rem;
      height: .26rem;
    }
    &__tag{
      position: absolute;
      top: .04rem;
      left: .46rem;
      min-width: .2rem;
      height: .2rem;
      line-height: .2rem;
      border-radius: 0.1rem;
      font-size: .12rem;
      text-align: center;
      background-color: #e93b3b;
      color: #fff;
      transform: scale(.5);
      transform-origin: left center;
    }
  }
  &__info{
    flex: 1;
    color: #333;
    font-size: .12rem;
    &__price{
      color: #e93b3b;
      font-size: .18rem;
    }
  }
  &__btn{
    width: .98rem;
    background-color:#4FB0F9;
    text-align: center;
    color: #fff;
    font-size: .14rem;
  }
}
</style>
