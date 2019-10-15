<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li class="menu-item" v-for="(good, index) in goods" :key="good.name" :class="{'current':currentIndex===index}" @click="selectMenu(index)">
          <span class="text">
            <icon v-if="good.type > 0" :size="12" :typeArr="[1, good.type]"></icon>
            {{good.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li v-for="item in goods" :key="item.name" class="food-list-hook">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li @click="selectFood(food)" v-for="food in item.foods" :key="food.name" class="food-item">
              <div class="icon">
                <img width="57" height="57" :src="food.icon" alt="">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="now">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :selectFoods="selectFoods" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice"></shopcart>
    <food ref="food" :food="selectedFood"></food>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import icon from '../icon/icon.vue'
import shopcart from '../shopcart/shopcart.vue'
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import food from '../food/food.vue'

const ERR_OK = 0
export default {
  data () {
    return {
      seller: {},
      goods: [],
      listHeight: [],
      scrollY: 0,
      selectedFood: {}
    }
  },
  created () {
    this.$http.get('/api/goods').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.goods = response.data
        this.$nextTick(() => {
          this._initScroll()
          this._scrollHeight()
        })
      }
    })
    this.$http.get('/api/seller').then((response) => {
      response = response.body
      if (response.errno === ERR_OK) {
        this.seller = response.data
      }
    })
  },
  computed: {
    currentIndex () {
      for (var i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i]
        let height2 = this.listHeight[i + 1]
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return i
        }
      }
      return 0
    },
    selectFoods () {
      let foods = []
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food)
          }
        })
      })
      return foods
    }
  },
  components: {
    icon,
    shopcart,
    cartcontrol,
    food
  },
  methods: {
    selectMenu (index) {
      this.scrollY = this.listHeight[index]
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
      let el = foodList[index]
      this.foodsScroll.scrollToElement(el, 300)
    },
    _initScroll () {
      this.menuScroll = new BScroll(this.$refs.menuWrapper, {
        click: true
      })
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        click: true,
        probeType: 3
      })
    },
    _scrollHeight () {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
      let height = 0
      this.listHeight.push(height)
      for (var i = 0; i < foodList.length; i++) {
        let item = foodList[i]
        height += item.clientHeight
        this.listHeight.push(height)
      }

      this.foodsScroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    selectFood (food) {
      this.selectedFood = food
      this.$refs.food.show()
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
.goods
  display flex
  position absolute
  top 178px
  bottom 46px
  width 100%
  overflow hidden
  .menu-wrapper
    flex 0 0 80px
    width 80px
    background #f3f5f7
    .menu-item
      display table
      height 54px
      width 56px
      line-height 14px
      font-size 12px
      padding 0 12px
      &.current
        position relative
        z-index 10
        background #fff
        margin -1px
        font-weight 700
        .text
          border-bottom 0
      .icon
        margin-right 2px
      .text
        display table-cell
        font-size 12px
        width 56px
        vertical-align middle
        border-bottom 0.5px solid rgba(7, 17, 27, 0.1)
  .foods-wrapper
    flex 1
    .title
      height 26px
      padding-left 14px
      line-height 26px
      font-size 12px
      background #f3f5f7
      border-left 2px solid #d9dde1
      color rgb(147, 153, 159)
    .food-item
      position relative
      display flex
      margin 18px
      padding-bottom 18px
      border-bottom 0.5px solid rgba(7, 17, 27, 0.1)
      &:last-child
        margin-bottom 0
        border-bottom 0
      .icon
        flex 0 0 57
        margin-right 10px
      .content
        flex 1
        .name
          margin 2px 0 8px 0
          height 14px
          line-height 14px
          font-size 14px
          color rgb(7, 17, 27)
        .desc, .extra
          margin-bottom 8px
          font-size 10px
          line-height 10px
          color rgb(147, 153, 159)
        .desc
          line-height 12px
        .extra
          margin-bottom 0
          .count
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
          right 0
          bottom 12px
</style>
