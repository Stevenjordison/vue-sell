<template>
  <span class="iconMap" :class="classMap[iconType],map_n[iconTheme]"></span>
</template>
/usr/bin/ssh-add

来自: http://man.linuxde.net/ssh-add
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
