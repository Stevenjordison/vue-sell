<template>
  <span class="iconMap" :class="[classMap[iconType], map_n[iconTheme]]"></span>
</template>

<script type="text/ecmascript-6">
  export default {
    props: {
      iconType: Number,
      iconTheme: Number
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.map_n = ['map_a', 'map_b'];
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"
  .iconMap
    display inline-block
    background-size 100% 100%
    background-repeat no-repeat
    &.map_a
      &.decrease
        bg-image('decrease_2')
      &.discount
        bg-image('discount_2')
      &.guarantee
        bg-image('guarantee_2')
      &.invoice
        bg-image('invoice_2')
      &.special
        bg-image('special_2')
    &.map_b
      &.decrease
        bg-image('decrease_4')
      &.discount
        bg-image('discount_4')
      &.guarantee
        bg-image('guarantee_4')
      &.invoice
        bg-image('invoice_4')
      &.special
        bg-image('special_4')
</style>
