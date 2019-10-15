<template>
  <div class="header">
    <!-- 分为上下两个部分
         上边是商家信息
            上边又分为两个部分 头像和商家信息
         下边是商家公告
     -->
     <div class="content-wrapper">
       <div class="avatar">
         <img width="64" height="64" :src="seller.avatar" alt="">
       </div>
       <div class="content">
         <div class="title">
           <span class="brand"></span>
           <span class="name">{{seller.name}}</span>
         </div>
         <div class="description">
           {{seller.description}}/{{seller.deliveryTime}}分钟送达
         </div>
         <div class="support" v-if="seller.supports">
           <!-- <span class="icon" :class="classMap[seller.supports[0].type]"></span> -->
           <icon :size="12" :typeArr="[0, seller.supports[0].type]"></icon>
           <span class="text">{{seller.supports[0].description}}</span>
         </div>
       </div>
       <div class="support-count" v-if="seller.supports" @click="showDetail">
         <span class="count">{{seller.supports.length}}个</span>
         <i class="icon-keyboard_arrow_right"></i>
       </div>
     </div>
     <div class="bulletin-wrapper" @click="showDetail">
       <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
       <i class="icon-keyboard_arrow_right"></i>
     </div>
     <div class="background">
       <img :src="seller.avatar" width="100%" height="100%" alt="">
     </div>
     <div v-show="detailShow" class="detail">
       <div class="detail-wrapper">
         <div class="detail-main">
           <h1 class="name">{{seller.name}}</h1>
           <div class="star-wrapper">
             <star :size="48" :score="seller.score"></star>
           </div>
           <div class="title">
             <div class="line"></div>
             <div class="text">优惠信息</div>
             <div class="line"></div>
           </div>
           <ul class="supports" v-if="seller.supports">
             <li class="item-support" v-for="support in seller.supports" :key="support.description">
               <icon :size="16" :typeArr="[0, support.type]"></icon>
               <span class="text">{{support.description}}</span>
             </li>
           </ul>
           <div class="title">
             <div class="line"></div>
             <div class="text">商家公告</div>
             <div class="line"></div>
           </div>
           <div class="bulletin">
             <p class="content">{{seller.bulletin}}</p>
           </div>
         </div>
       </div>
       <div class="detail-close" @click="hideDetail">
         <i class="icon-close"></i>
       </div>
     </div>
  </div>
</template>

<script>
import star from '../star/star.vue'
import icon from '../icon/icon.vue'
export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail () {
      this.detailShow = true
    },
    hideDetail () {
      this.detailShow = false
    }
  },
  created () {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  components: {
    star,
    icon
  }
}
</script>

<style lang="stylus">
@import '../../common/stylus/icon.styl'
.header
  position relative
  color #fff
  overflow hidden
  .content-wrapper
    position relative
    padding 24px 12px 18px 24px
    background-color rgba(7, 17, 27, 0.5)
    .avatar
      display inline-block
      vertical-align top
      img
        border-radius 2px
    .content
      margin-left 16px
      display inline-block
      font-size 14px
      .title
        margin 2px 0 8px 0
        .brand
          display inline-block
          vertical-align top
          width 30px
          height 18px
          background-image url('brand@2x.png')
          background-size 30px 18px
          background-repeat no-repeat
        .name
          margin-left 6px
          font-size 16px
          line-height 18px
          font-weight blod
      .description
        margin-bottom 10px
        font-size 12px
        font-weight 200
        line-height 12px
      .support
        font-size 10px
        line-height 12px
        font-weight 200
        .text
          font-size 10px
          line-height 12px
          font-weight 200
    .support-count
      position absolute
      bottom 14px
      right 12px
      padding 0 8px
      background-color rgba(0, 0, 0, 0.2)
      height 24px
      text-align center
      line-height 24px
      border-radius 14px
      .count
        vertical-align top
        font-size 10px
      .icon-keyboard_arrow_right
        line-height 24px
        margin-left 2px
        font-size 10px

  .bulletin-wrapper
    position relative
    height 28px
    line-height 28px
    padding 0 12px
    background-color rgba(7, 17, 27, 0.2)
    white-space nowrap
    overflow hidden
    text-overflow ellipsis
    .bulletin-title
      display inline-block
      vertical-align top
      margin-top 8px
      width 22px
      height 12px
      background-image url('bulletin@2x.png')
      background-size 22px 12px
      background-repeat no-repeat
    .bulletin-text
      vertical-align top
      margin 0 4px
      padding-right 3px
      font-size 10px
      font-weight 200
    .icon-keyboard_arrow_right
      position absolute
      right 12px
      top 8px
      font-size 10px
  .background
    width 100%
    height 100%
    z-index -1
    position absolute
    top 0
    left 0
    filter blur(10px)
  .detail
    position fixed
    top 0
    left 0
    z-index 100
    overflow auto
    width 100%
    height 100%
    background rgba(7, 17, 27, 0.8)
    .detail-wrapper
      min-height 90%
      width 100%
      .detail-main
        margin-top 64px
        padding-bottom 64px
        .name
          font-size 16
          font-weight 700
          line-height 16px
          text-align center
        .star-wrapper
          margin-top 18px
          padding 2px 0
          text-align center
        .title
          display flex
          width 80%
          margin 28px auto 24px auto
          .line
            flex 1
            position relative
            top -6px
            border-bottom 1px solid rgba(255, 255, 255, 0.2)
          .text
            padding 0 12px
            font-size 14px
            font-weight 700
        .supports
          width 80%
          margin 0 auto
          .item-support
            padding 0 12px
            margin-bottom 12px
            font-size 0
            &:last-child
              margin-bottom 0
            .text
              vertical-align top
              font-size 12px
              line-height 16px
              font-weight 200
        .bulletin
          margin 0 auto
          width 80%
          .content
            padding 0 12px
            line-height 24px
            font-size 12px
            font-weight 200

    .detail-close
      position relative
      width 32px
      height 32px
      margin -64px auto 0 auto
      clear both
      font-size 32px
</style>
