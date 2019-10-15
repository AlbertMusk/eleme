<template>
  <div class="star" :class="starType">
    <span class="star-item" v-for="itemClass in itemClasses" :key="itemClass.$index" :class="itemClass"></span>
  </div>
</template>
<script type="text/ecmascript-6">
const LENGTH = 5
const CLS_ON = 'on'
const CLS_HALF = 'half'
const CLS_OFF = 'off'

export default {
  props: {
    size: {
      type: Number,
      default: 24
    },
    score: {
      type: Number,
      default: 0
    }
  },
  computed: {
    starType () {
      return 'star-' + this.size
    },
    itemClasses () {
      let result = []
      let onStarLen = Math.floor(this.score)
      for (var i = 0; i < onStarLen; i++) {
        result.push('star-' + CLS_ON)
      }
      if ((this.score - onStarLen) > 0 && (this.score - onStarLen) < 0.5) {
        result.push('star-' + CLS_OFF)
      } else if ((this.score - onStarLen) >= 0.5 && (this.score - onStarLen) < 1) {
        result.push('star-' + CLS_HALF)
      }
      // if(result.length != LENGTH) {
      //   let len = LENGTH - result.length
      //   for(var i = 0; i < len; i++){
      //     result.push(CLS_OFF)
      //   }
      // }
      while (result.length < LENGTH) {
        result.push('star-' + CLS_OFF)
      }
      return result
    }
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
.star
  font-size 0
  .star-item
    display inline-block
    background-repeat no-repeat
  &.star-24
    .star-item
      width 10px
      height 10px
      margin-right 3px
      background-size 10px 10px
      &:last-child
        margin-right 0
      &.star-on
        background-image url('star24_on@2x.png')
      &.star-half
        background-image url('star24_half@2x.png')
      &.star-off
        background-image url('star24_off@2x.png')
  &.star-36
    .star-item
      width 15px
      height 15px
      margin-right 16px
      background-size 15px 15px
      &:last-child
        margin-right 0
      &.star-on
        background-image url('star36_on@2x.png')
      &.star-half
        background-image url('star36_half@2x.png')
      &.star-off
        background-image url('star36_off@2x.png')
  &.star-48
    .star-item
      width 20px
      height 20px
      margin-right 22px
      background-size 20px 20px
      &:last-child
        margin-right 0
      &.star-on
        background-image url('star48_on@2x.png')
      &.star-half
        background-image url('star48_half@2x.png')
      &.star-off
        background-image url('star48_off@2x.png')
</style>
