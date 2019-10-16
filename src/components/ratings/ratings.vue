<template>
<div class="ratings" ref="ratingsWrapper">
  <div class="ratings-content">
    <div class="overview">
      <div class="overview-left">
        <h1 class="score">{{seller.score}}</h1>
        <div class="title">综合评分</div>
        <div class="rank">高于周边商家{{seller.rankRate}}%</div>
      </div>
      <div class="overview-right">
        <div class="rating-service">
          <span class="text">服务态度</span>
          <star class="service-star" :score="seller.serviceScore" :size="36"></star>
          <span class="score">{{seller.serviceScore}}</span>
        </div>
        <div class="rating-food">
          <span class="text">食品评价</span>
          <star class="food-star" :score="seller.foodScore" :size="36"></star>
          <span class="score">{{seller.foodScore}}</span>
        </div>
        <div class="deliveryTime">
          <span class="text">送达时间</span>
          <span class="deli-time">{{seller.deliveryTime}}分钟</span>
        </div>
      </div>
    </div>
    <split></split>
    <div class="rating">
      <ratingselect @toggle="changeOnlyContent" @select="changeSelectType" :selectType="selectType" :onlyContent="onlyContent" :desc="desc" :ratings="ratings"></ratingselect>
      <rating :ratings="ratings" :selectType="selectType" :onlyContent="onlyContent"></rating>
    </div>
  </div>
</div>
</template>
<script type="text/ecmascript-6">
import star from '../star/star.vue'
import split from '../split/split.vue'
import ratingselect from '../ratingselect/ratingselect.vue'
import rating from '../rating/rating.vue'
import BScroll from 'better-scroll'

const ERR_OK = 0
const ALL = 2
// const POSITIVE = 1
// const NEGATIVE = 0

export default {
  data () {
    return {
      desc: {
        all: '全部',
        positive: '满意',
        negative: '不满意'
      },
      ratings: [],
      onlyContent: false,
      selectType: ALL
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  created () {
    this.$http.get('/api/ratings').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.ratings = response.data
      }
    })
    this.$nextTick(() => {
      if (!this.ratingsWrapper) {
        this.ratingsWrapper = new BScroll(this.$refs.ratingsWrapper, {
          click: true
        })
      } else {
        this.ratingsWrapper.refresh()
      }
    })
  },
  methods: {
    changeOnlyContent () {
      this.onlyContent = !this.onlyContent
    },
    changeSelectType (type) {
      this.selectType = type
    }
  },
  components: {
    star,
    split,
    ratingselect,
    rating
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
.ratings
  position absolute
  top 174px
  bottom 0
  left 0
  width 100%
  overflow hidden
  .overview
    display flex
    padding 18px 0
    .overview-left
      flex 0 0 137px
      width 137px
      border-right 1px solid rgba(7, 17, 27, 0.1)
      text-align center
      .score
        margin-bottom 6px
        line-height 28px
        font-size 24px
        color rgb(255, 153, 0)
      .title
        margin-bottom 8px
        line-height 12px
        font-size 12px
        color rgb(7, 17, 27)
      .rank
        padding-bottom 18px
        line-height 10px
        font-size 10px
        color rgb(147, 153, 159)
    .overview-right
      flex 1
      .rating-service, .rating-food, .deliveryTime
        padding 0 24px
        margin-bottom 8px
        .text
          display inline-block
          margin-right 12px
          line-height 18px
          font-size 12px
          color rgb(7, 17, 27)
        .service-star, .food-star
          display inline-block
          margin-right 12px
          line-height 18px
          vertical-align top
        .score
          vertical-align top
          font-size 12px
          line-height 18px
          color rgb(255, 153, 0)
        .deli-time
          font-size 12px
          line-height 16px
          color rgb(147, 153, 159)
</style>
