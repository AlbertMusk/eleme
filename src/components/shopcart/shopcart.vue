<template>
<div class="shopcart-wrapper">
  <div class="shopcart">
    <div class="content">
      <div class="content-left" @click="toggleList">
        <div class="logo-wrapper">
          <div class="logo" :class="{'highlight':totalCount > 0}">
            <i class="icon-shopping_cart" :class="{'highlight':totalCount > 0}"></i>
          </div>
          <div v-if="totalCount > 0" class="num">{{totalCount}}</div>
        </div>
        <div class="price" :class="{'highlight':totalPrice > 0}">￥{{totalPrice}}元</div>
        <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
      </div>
      <div class="content-right">
        <div :class="{'highlight':totalPrice>=minPrice}" class="pay">
          {{payDesc}}
        </div>
      </div>
      <div class="shopcart-list fold-transition" v-show="listShow" transition="fold">
        <div class="list-header">
          <h1 class="title">购物车</h1>
          <span class="empty" @click="emptyCart">清空</span>
        </div>
        <div class="list-content" ref="contentWrapper">
          <ul>
            <li class="food" v-for="food in selectFoods" :key="food.name">
              <h1 class="food-name">{{food.name}}</h1>
              <span class="price">￥{{food.price*food.count}}</span>
              <div class="cartcontrol-wrapper">
                <cartcontrol :food="food"></cartcontrol>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
  <div class="list-mask" v-show="listShow"></div>
</div>
</template>
<script type="text/ecmascript-6">
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import BScroll from 'better-scroll'
export default {
  data () {
    return {
      fold: true
    }
  },
  props: {
    minPrice: {
      type: Number
    },
    deliveryPrice: {
      type: Number
    },
    selectFoods: {
      type: Array,
      default () {
        return []
      }
    }
  },
  components: {
    cartcontrol
  },
  computed: {
    totalPrice () {
      let total = 0
      this.selectFoods.forEach((food) => {
        total += (food.price * food.count)
      })
      return total
    },
    totalCount () {
      let count = 0
      this.selectFoods.forEach((food) => {
        count += food.count
      })
      return count
    },
    payDesc () {
      if (this.totalPrice >= this.minPrice) {
        return '去结算'
      } else if (this.totalPrice > 0 && this.totalPrice < this.minPrice) {
        return `还差￥${(this.minPrice - this.totalPrice)}元起送`
      } else if (this.totalPrice === 0) {
        return `￥${this.minPrice}元起送`
      } else {
        return undefined
      }
    },
    listShow () {
      if (!this.totalCount) {
        this.fold = true
        return false
      }
      let show = !this.fold
      if (show) {
        this.$nextTick(() => {
          this.contentScroll = new BScroll(this.$refs.contentWrapper, {})
        })
      }
      return show
    }
  },
  methods: {
    toggleList () {
      if (!this.totalCount) {
        return
      }
      this.fold = !this.fold
    },
    emptyCart () {
      if (this.selectFoods) {
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
      }
      this.selectFoods = []
    }
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
.shopcart
  position fixed
  left 0
  bottom 0
  z-index 100
  height 48px
  width 100%
  background #000
  .content
    display flex
    background #141d27
    font-size 0
    .content-left
      flex 1
      .logo-wrapper
        display inline-block
        position relative
        top -10px
        margin 0 12px
        padding 6px
        width 56px
        height 56px
        box-sizing border-box
        vertical-align top
        border-radius 50%
        background #141d27
        .logo
          width 100%
          height 100%
          border-radius 50%
          background #2b343c
          text-align center
          .icon-shopping_cart
            font-size 24px
            color #80858a
            line-height 44px
            &.highlight
              color #fff
          &.highlight
            background rgb(0, 160, 220)
        .num
          position absolute
          top 0
          right 0
          width 24px
          height 16px
          line-height 16px
          text-align center
          border-radius 16px
          font-size 9px
          font-weight 700
          color #fff
          background rgb(240, 20, 20)
          box-shadow 0 4px 8px 0 rgba(0, 0, 0, 0.4)
      .price
        display inline-block
        vertical-align top
        line-height 24px
        margin-top 12px
        box-sizing border-box
        padding-right 12px
        border-right 1px solid rgba(255,255,255,0.1)
        font-size 16px
        font-weight 700
        color rgba(255,255,255,0.4)
        &.highlight
          color #fff
      .desc
        display inline-block
        vertical-align top
        line-height 24px
        margin-top 12px
        margin-left 12px
        font-size 12px
        color rgba(255,255,255,0.4)
    .content-right
      flex 0 0 105px
      width 105px
      background #2b333b
      .pay
        height 48px
        line-height 48px
        text-align center
        color rgba(255,255,255,0.4)
        font-size 12px
        &.highlight
          background #00b43c
          color #fff
  .shopcart-list
    position absolute
    top 0
    left 0
    z-index -1
    width 100%
    &.fold-transition
      transition all 0.5s
      transform translate3d(0, -100%, 0)
    &.fold-enter, &.fold-leave
      transform translate3d(0, 0, 0)
    .list-header
      height 40px
      line-height 40px
      padding 0 18px
      background #F3F5F7
      border-bottom 1px solid rgba(7, 17, 27, 0.2)
      .title
        float left
        font-size 14px
        color rgb(7, 17, 27)
      .empty
        float right
        font-size 12px
        color rgb(0, 160, 220)
    .list-content
      padding 0 18px
      max-height 217px
      background #FFF
      overflow hidden
      .food
        position relative
        padding 12px 0
        box-sizing border-box
        border-bottom 1px solid rgba(7, 17, 27, 0.1)
        .food-name
          line-height 24px
          font-size 14px
          color rgb(7, 17, 27)
        .price
          position absolute
          right 90px
          bottom 12px
          line-height 24px
          font-size 14px
          font-weight 700
          color rgb(240, 20, 20)
        .cartcontrol-wrapper
          position absolute
          right 0
          bottom 6px
.list-mask
  position fixed
  top 0
  left 0
  width 100%
  height 100%
  z-index 40
  backdrop-filter: blur(10px)
  background rgba(7, 17, 27, 0.6)
</style>
