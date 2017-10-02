<template>
  <transition name="move">
    <div v-show="showFlag" class="food" ref="foodInfo">
      <div class="food-content">
        <div class="image-header">
          <img :src="food.image">
          <div class="back" @click="hide">
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
            <span class="old" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
          </div>
        </div>
        <div class="cartcontrol-wrapper">
          <v-cartControl :food="food"></v-cartControl>
        </div>
        <div @click.stop.prevent="addFirst" class="buy" v-show="!food.count || food.count === 0">加入购物车</div>
      </div>
    </div>
  </transition>
</template>

<script>
import BScroll from 'better-scroll'
import CartControl from 'components/cartcontrol/cartcontrol'
import Vue from 'vue'

export default {
  props: {
    food: {
      type: Object
    }
  },
  components: {
    'v-cartControl': CartControl
  },
  data() {
    return {
      showFlag: false
    }
  },
  methods: {
    initShow() {
      this.showFlag = true
      this.$nextTick(() => {
        console.log(this.scroll)
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.foodInfo, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      })
    },
    hide() {
      this.showFlag = false
    },
    addFirst(event) {
      if (!event._constructed) {
        return
      }
      this.$root.eventHub.$emit('cart.add', event.target)
      Vue.set(this.food, 'count', 1)
    }
  }
}
</script>

<style lang="stylus" scoped>
  @import "../../common/stylus/mixin.styl"

  .food
    position fixed
    top 0
    left 0
    bottom 48px
    z-index 30
    width 100%
    background #fff
    transition all 0.3s
    &.move-enter-active
      transform:translate3d(0,0,0)
    &.move-enter,&.move-leave-to
      transform:translate3d(100%,0,0)
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
      padding 18px
      .title
        line-height 14px
        margin-bottom 8px
        font-size 14px
        font-weight 700
        color rgb(7,17,27)
      .detail
        margin-bottom 18px
        line-height 10px
        font-size 0
        height 10px
        .sell-count,.rating
          font-size 10px
          color rgb(147,153,159)
        .sell-count
          margin-right 12px
      .price
        line-height 24px
        font-weight 700
        .now
          margin-right 8px
          font-size 14px
          color rgb(240, 20, 20)
        .old
          text-decoration line-through
          font-size 10px
          color rgb(147, 153, 159)
    .cartcontrol-wrapper
      position absolute
      right 12px
      bottom 12px
    .buy
      position absolute
      right 18px
      bottom 18px
      z-index 10
      height 24px
      line-height 24px
      padding 0 12px
      box-sizing border-box
      border-radius 12px
      font-size 10px
      color #fff
      background rgb(0,160,220)
      
</style>
