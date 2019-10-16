<template>
<div class="seller" ref="sellerWrapper">
  <div class="seller-content">
    <div class="seller-detail">
      <span class="seller-name">{{seller.name}}</span>
      <star :size="36" :score="seller.score"></star>
      <span class="level">(661)</span>
      <span class="mon-sell-content">月售{{seller.sellCount}}单</span>
    </div>
    <div class="service">
      <div class="min-price">
        <span class="title">起送价</span>
        <span class="text">{{seller.minPrice}}</span>元
      </div>
      <div class="delivery-price">
        <span class="title">商家配送</span>
        <span class="text">{{seller.deliveryPrice}}</span>元
      </div>
      <div class="delivery-time">
        <span class="title">平均配送时间</span>
        <span class="text">{{seller.deliveryTime}}</span>分钟
      </div>
    </div>
    <split></split>
    <div class="bulletin">
      <div class="title">公告与活动</div>
      <p class="content">{{seller.bulletin}}</p>
    </div>
    <div class="supports">
      <ul>
        <li v-show="seller.supports" class="support" v-for="support in seller.supports" :key="support.description">
          <icon :size="16" :typeArr="[1, support.type]"></icon>
          <span class="text">{{support.description}}</span>
        </li>
      </ul>
    </div>
    <split></split>
    <div class="pics">
      <div class="title">商家实景</div>
      <div class="pic-wrapper" ref="picWrapper">
        <ul ref="picList">
          <li class="pic" v-for="pic in seller.pics" v-show="seller.pics" :key="pic">
            <img :src="pic" alt="" width="120" height="90">
          </li>
        </ul>
      </div>
    </div>
    <split></split>
    <div class="seller-infos">
      <h1 class="title">商家信息</h1>
      <ul>
        <li class="info-item" v-for="info in seller.infos" :key="info">{{info}}</li>
      </ul>
    </div>
  </div>
</div>
</template>
<script type="text/ecmascript-6">
import star from '../star/star.vue'
import split from '../split/split.vue'
import BScroll from 'better-scroll'
import icon from '../icon/icon.vue'

export default {
  props: {
    seller: {
      type: Object,
      default () {}
    }
  },
  components: {
    star,
    split,
    icon
  },
  created () {
    this.$nextTick(() => {
      this.sellerScroll = new BScroll(this.$refs.sellerWrapper, {})
      if (this.seller.pics) {
        let picWidth = 120
        let margin = 6
        let width = (picWidth + margin) * this.seller.pics.length - margin
        this.$refs.picList.style.width = `${width}px`
        this.picScroll = new BScroll(this.$refs.picWrapper, {
          scrollX: true,
          eventPassthrough: 'vertical'
        })
      }
    })
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
.seller
  position absolute
  top 178px
  bottom 0
  left 0
  width 100%
  overflow hidden
  .seller-content
    padding 18px 0
    .seller-detail
      margin 0 18px
      border-bottom 1px solid rgba(7, 17, 27, 0.1)
      .seller-name
        display block
        font-size 14px
        line-height 14px
        color rgb(7, 17, 27)
        margin-bottom 8px
      .star
        display inline-block
        margin-bottom 18px
      .level
        display inline-block
        margin 0 12px 0 18px
        font-size 10px
        color rgb(77, 85, 93)
        line-height 18px
      .mon-sell-content
        display inline-block
        font-size 10px
        color rgb(77, 85, 93)
        line-height 18px
    .service
      display flex
      padding 18px
      font-size 10px
      line-height 10px
      .min-price, .delivery-price, .delivery-time
        flex 1
        text-align center
        border-right 1px solid rgba(7, 17, 27, 0.1)
        &:last-child
          border 0
        .title
          display block
          margin-bottom 4px
          color rgb(147, 153, 159)
        .text
          font-size 24px
          line-height 24px
          font-weight 200
          color rgb(7, 17, 27)
    .bulletin
      padding 18px
      padding-bottom 12px
      .title
        font-size 20px
        line-height 20px
        color rgb(7, 17, 27)
        margin-bottom 8px
      .content
        padding 0 12px
        font-size 12px
        font-weight 200
        color rgb(240, 20, 20)
        line-height 24px
    .supports
      margin 0 18px
      .support
        border-top 1px solid rgba(7, 17, 27, 0.1)
        padding 16px 12px
        .text
          vertical-align top
          margin-left 6px
          font-size 12px
          font-weight 200
          line-height 16px
          color rgb(7, 17, 27)
    .pics
      padding 18px
      .title
        font-size 20px
        line-height 20px
        color rgb(7, 17, 27)
        margin-bottom 12px
      .pic-wrapper
        width 100%
        overflow hidden
        white-space nowrap
        .pic
          display inline-block
          width 120px
          height 90px
          margin-right 6px
          &:last-child
            margin-right 0
    .seller-infos
      margin 18px
      .title
        font-size 20px
        line-height 20px
        color rgb(7, 17, 27)
        margin-bottom 12px
      .info-item
        border-top 1px solid rgba(7, 17, 27, 0.1)
        padding 16px 12px
        margin-left 6px
        font-size 12px
        font-weight 200
        line-height 16px
        color rgb(7, 17, 27)
</style>
