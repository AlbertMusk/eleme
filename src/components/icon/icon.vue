<template>
  <span class="icon-wrapper" :class="classSize">
    <span class="icon" :class="classType"></span>
  </span>
</template>
<script type="text/ecmascript-6">
export default {
  props: {
    typeArr: {
      type: Array
    },
    size: {
      type: Number,
      default: 12
    }
  },
  data () {
    return {
      classMap: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    }
  },
  computed: {
    classSize () {
      return 'icon-' + this.size
    },
    classType () {
      return this.classMap[this.typeArr[1]] + '-' + this.typeArr[0]
    }
  }
}
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/icon.styl'
.icon-16
  width 16px
  height 16px
  .icon
    width 16px
    height 16px
    display inline-block
    margin-right 6px
    background-size 16px 16px
    background-repeat no-repeat
    vertical-position top
    &.decrease-0
      background-image url('decrease_2@2x.png')
    &.discount-0
      background-image url('discount_2@2x.png')
    &.guarantee-0
      background-image url('guarantee_2@2x.png')
    &.invoice-0
      background-image url('invoice_2@2x.png')
    &.special-0
      background-image url('special_2@2x.png')
    &.decrease-1
      background-image url('decrease_3@3x.png')
    &.discount-1
      background-image url('discount_3@3x.png')
    &.guarantee-1
      background-image url('guarantee_3@3x.png')
    &.invoice-1
      background-image url('invoice_3@3x.png')
    &.special-1
      background-image url('special_3@3x.png')
.icon-12
  width 12px
  height 12px
  .icon
    display inline-block
    vertical-align top
    width 12px
    height 12px
    margin-right 4px
    background-size 12px 12px
    background-repeat no-repeat
    &.decrease-0
      background-image url('decrease_1@2x.png')
    &.discount-0
      background-image url('discount_1@2x.png')
    &.guarantee-0
      background-image url('guarantee_1@2x.png')
    &.invoice-0
      background-image url('invoice_1@2x.png')
    &.special-0
      background-image url('special_1@2x.png')
    &.decrease-1
      background-image url('decrease_3@2x.png')
    &.discount-1
      background-image url('discount_3@2x.png')
    &.guarantee-1
      background-image url('guarantee_3@2x.png')
    &.invoice-1
      background-image url('invoice_3@2x.png')
    &.special-1
      background-image url('special_3@2x.png')
</style>
