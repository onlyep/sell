<template>
<div class="cartcontrol">
  <transition name="fade">
    <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click.stop.prevent="decreaseCart"></div>
  </transition>
  <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
  <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
</div>
</template>

<script type="text/ecmascript-6">
import Vue from 'vue'

export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart(event) {
      if (!event._constructed) { // 阻止pc端点击后执行两次
        return
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      this.$root.eventHub.$emit('cart.add', event.target)
    },
    decreaseCart(event) {
      if (!event._constructed) { // 阻止pc端点击后执行两次
        return
      }
      if (this.food.count) {
        this.food.count--
      } else {
        return
      }
    }
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size: 0
    .cart-decrease, .cart-add
      display: inline-block
      padding: 6px
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
    .cart-count
      display: inline-block
      vertical-align: top
      width: 12px
      padding-top: 6px
      line-height: 24px
      text-align: center
      font-size: 10px
      color: rgb(147, 153, 159)
    .cart-add
      display: inline-block
    .fade-enter-active
      transition: all .4s ease
    .fade-leave-active
      transition: all .4s ease
    .fade-enter, .fade-leave-to
      transform: translate3d(30px,0,0) rotate(360deg)
      opacity: 0
</style>
