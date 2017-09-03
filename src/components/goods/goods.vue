<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="goods-wrapper"></div>
  </div>
</template>

<script type="text/ecmascript-6">

const ERR_OK = 0

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data() {
    return {
      goods: []
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']

    this.$http.get('/api/goods').then(response => {
      // get body data
      response = response.body
      if (response.errno === ERR_OK) {
        console.log(response)
        this.goods = response.data
        return
      }
    }, response => {
      // error callback
    })
  }
}
</script>

<style lang="stylus" rel="stylesheet/stylus">

@import "../../common/stylus/mixin.styl"

  .goods
    display flex
    width 100%
    position absolute
    top 174px
    bottom 46px
    .menu-wrapper
      flex 0 0 80px
      width 80px
      background #f3f5f7
      .menu-item
        display table
        width 56px
        height 54px
        padding 0 12px
        .icon
          display inline-block
          vertical-align top
          width 12px
          height 12px
          margin-right 2px
          background-size 12px 12px
          background-repeat no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
        .text
          display table-cell
          width 56px
          vertical-align middle
          font-size 12px
          line-height 14px
          border-1px(rgba(7,17,27,.1))
    .goods-wrapper
      flex 1
</style>
