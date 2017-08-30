<template>
  <div id="app">
    <v-header></v-header>
    <div class="tab">
      <div class="tab-item">商品</div>
      <div class="tab-item">评价</div>
      <div class="tab-item">商家</div>
    </div>
    <div class="content">
      i am content
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header'
  export default {
    components: {
      'v-header': header
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">

</style>
