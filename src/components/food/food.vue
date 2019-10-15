<template>
<div class="food" v-show="showFlag" ref="foodWrapper">
  <div class="food-content">
    <div class="image-header">
      <img :src="food.image" alt="">
      <div class="back" @click.stop.prevent="back()">
        <i class="icon-arrow_lift"></i>
      </div>
    </div>
    <div class="content">
      <h1 class="title">{{food.name}}</h1>
      <div class="detail">
        <span class="sell-count">月售{{food.sellCount}}份</span>
        <span class="rating">好评率{{food.rating}}%</span>
      </div>
      <div class="price">
        <span class="now">￥{{food.price}}</span>
        <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
      </div>
      <div class="cartcontrol-wrapper">
          <cartcontrol :food="food"></cartcontrol>
        </div>
        <div @click.stop.prevent="addFirst()" class="buy" v-show="!food.count || food.count===0">加入购物车</div>
    </div>
    <split></split>
    <div class="info" v-show="food.info">
      <h1 class="title">商品信息</h1>
      <p class="text">{{food.info}}</p>
    </div>
    <split></split>
    <div class="rating">
      <h1 class="title">商品评价</h1>
      <ratingselect @toggle="changeOnlyContent" @select="changeSelectType" :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="food.ratings"></ratingselect>
      <rating :ratings="food.ratings" :selectType="selectType" :onlyContent="onlyContent"></rating>
    </div>
  </div>
</div>
</template>
<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import split from '../split/split.vue'
import ratingselect from '../ratingselect/ratingselect.vue'
import rating from '../rating/rating.vue'

// const POSITIVE = 0
// const NEGATIVE = 1
const ALL = 2

export default {
  data () {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: false,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    show () {
      this.showFlag = true
      this.selectType = 2
      this.onlyContent = false
      if (!this.foodScroll) {
        this.foodScroll = new BScroll(this.$refs.foodWrapper, {
          click: true
        })
      } else {
        this.foodScroll.refresh()
      }
    },
    back () {
      this.showFlag = false
    },
    addFirst () {
      this.$set(this.food, 'count', 1)
    },
    changeSelectType (type) {
      this.selectType = type
    },
    changeOnlyContent () {
      this.onlyContent = !this.onlyContent
    }
  },
  components: {
    cartcontrol,
    split,
    ratingselect,
    rating
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/icon.styl'
.food
  overflow hidden
  position fixed
  top 0
  left 0
  bottom 48px
  width 100%
  z-index 30
  background #fff
  .image-header
    position relative
    width 100%
    height 0
    padding-top 100%
    img
      position absolute
      top 0
      left 0
      width 100%
      height 100%
    .back
      position absolute
      top 10px
      left 0
      .icon-arrow_lift
        display block
        padding 10px
        font-size 20px
        color #fff

  .content
    position relative
    padding 18px
    .title
      line-height 14px
      margin-bottom 8px
      font-size 14px
      font-weight 700
      color rgb(7, 17, 27)
    .detail
      margin-bottom 18px
      line-height 10px
      height 10px
      font-size 0
      .sell-count, .rating
        font-size 10px
        color rgb(147, 153, 159)
      .sell-count
        margin-right 12px
    .price
      .now
        font-size 14px
        color rgb(240, 20, 20)
        font-weight 700
        line-height 24px
        margin-right 8px
      .old
        font-size 10px
        color rgb(147, 153, 159)
        font-weight normal
        line-height 24px
        text-decoration line-through
  .cartcontrol-wrapper
    position absolute
    right 12px
    bottom 12px
  .buy
    position absolute
    right 18px
    bottom 18px
    padding 0 12px
    line-height 24px
    box-sizing border-box
    border-radius 12px
    color #fff
    background rgb(0, 160, 220)
    z-index 10
    font-size 10px
  .info
    padding 18px
    .title
      line-height 14px
      margin-bottom 6px
      font-size 14px
      color rgb(7, 17, 27)
    .text
      line-height 24px
      padding 0 8px
      font-size 12px
      color rgb(77, 85, 93)
  .rating
    padding-top 18px
    .title
      line-height 14px
      margin-left 18px
      font-size 14px
      color rgb(7, 17, 27)
</style>
