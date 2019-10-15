<template>
<div class="rating-wrapper">
  <ul v-show="ratings && ratings.length">
    <li v-show="needShow(rating.rateType, rating.text)" v-for="rating in ratings" :key="rating.rateTime" class="rating-item">
      <div class="user">
        <span class="name">{{rating.username}}</span>
        <img width="12" height="12" class="avatar" :src="rating.avatar" alt="">
      </div>
      <div class="time">{{rating.rateTime | formatTime}}</div>
      <p class="text">
        <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>{{rating.text}}
      </p>
    </li>
  </ul>
  <div class="no-rating" v-show="!ratings || !ratings.length">
    暂无评价
  </div>
</div>
</template>
<script type="text/ecmascript-6">
const ALL = 2
export default {
  props: {
    ratings: {
      type: Array
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    selectType: {
      type: Number,
      default: ALL
    }
  },
  methods: {
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    }
  },
  filters: {
    formatTime: (time) => {
      let date = new Date(time)
      return `${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()} ${date.getHours() + 1}:${date.getMinutes()}`
    }
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
.rating-wrapper
  padding 0 18px
  .rating-item
    position relative
    padding 16px 0
    border-bottom 1px solid rgba(7, 17, 27, 0.1)
    .user
      position absolute
      right 0
      top 16px
      font-size 0
      line-height 12px
      .name
        display inline-block
        vertical-algin top
        font-size 10px
        margin-right 6px
        color rgb(147, 153, 159)
      .avatar
        border-radius 50%
        vertical-algin top
    .time
      margin-bottom 6px
      line-height 12px
      font-size 10px
      color rgb(147, 153, 159)
    .text
      line-height 16px
      font-size 12px
      color rgb(7, 17, 27)
    .icon-thumb_up, .icon-thumb_down
      line-height 16px
      margin-right 14px
      font-size 12px
    .icon-thumb_up
      color rgb(0, 160, 220)
    .icon-thumb_down
      color rgb(147, 153, 159)
</style>
