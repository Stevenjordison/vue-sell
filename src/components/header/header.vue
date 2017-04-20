<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64px" height="64px" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div class="support">
          <icon-map :iconType="seller.supports[0].type" :iconTheme="0"></icon-map>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div class="support-count" v-if="seller.supports" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%">
    </div>
    <transition name="fade">
      <div v-show="detailShow" class="detail">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">

            <!--detail-->
            <h1 class="name">{{seller.name}}</h1>
            <div class="star-wrapper">
              <star :size="48" :score="seller.score"></star>
            </div>
            <bg-title name="优惠信息"></bg-title>
            <ul v-if="seller.supports" class="supports">
              <li v-for="(item,index) in seller.supports" class="supports-item">
                <icon-map :iconType="seller.supports[index].type" :iconTheme="1"></icon-map>
                <span class="text">{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <bg-title name="商家公告"></bg-title>
            <div class="bulletin">
              <p class="content">{{seller.bulletin}}</p>
            </div>
          </div>
        </div>
        <div class="detail-close" @click="hideDetail"><i class="icon-close"></i></div>
      </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from 'components/star/star';
  import bgTitle from 'components/header/title';
  import iconMap from 'components/iconMap/status-icon';

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        detailShow: false
      };
    },
    methods: {
      showDetail() {
        this.detailShow = true;
      },
      hideDetail() {
        this.detailShow = false;
      }

    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      star,
      bgTitle,
      iconMap
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"

  .header
    position: relative
    color: #fff
    background rgba(7, 17, 27, .5)
    overflow: hidden
    .content-wrapper
      position relative
      padding 24px 12px 18px 24px
      font-size 0
      .avatar
        display inline-block
        img
          border-radius 2px
      .content
        display inline-block
        margin-left 16px
        .title
          margin 2px 0 8px 0
          .brand
            display inline-block
            vertical-align top
            width 30px
            height 18px
            bg-image('brand')
            background-size 30px 18px
            background-repeat no-repeat
          .name
            margin-left 6px
            font-size 16px
            line-height 18px
            font-weight bold
        .description
          margin-bottom 10px
          line-height 12px
          font-size 12px
        .support
          .iconMap
            vertical-align top
            width 12px
            height 12px
          .text
            line-height 16px
            font-size 12px
      .support-count
        position absolute
        height 24px
        line-height 24px
        padding 0 8px
        bottom 14px
        right 12px
        border-radius 14px
        background rgba(0, 0, 0, .2)
        text-align: center
        color: #fff
        .count
          vertical-align: top
          font-size 10px
        .icon-keyboard_arrow_right
          margin-left 2px
          line-height: 24px
          font-size 10px

    .bulletin-wrapper
      position: relative
      height: 28px
      line-height: 28px
      padding 0 22px 0 12px
      white-space nowrap
      overflow: hidden;
      text-overflow ellipsis
      background-color rgba(7, 17, 27, .2)
      .bulletin-title
        display inline-block
        vertical-align: top
        margin-top 8px
        width: 22px;
        height: 12px;
        bg-image('bulletin')
        background-size 22px 12px
        background-repeat no-repeat
      .bulletin-text
        vertical-align: top
        margin 0 4px
        font-size 10px
      .icon-keyboard_arrow_right
        position: absolute
        font-size 10px
        right 12px
        top 8px
    .background
      position: absolute
      top 0
      left 0
      width 100%
      height 100%
      z-index -1
      filter blur(10px)
    .detail
      position: fixed
      z-index 999
      top: 0
      left: 0
      width 100%
      height 100%
      overflow: auto
      backdrop-filter: blur(10px)
      opacity 1
      background rgba(7, 17, 27, .8)
      &.fade-enter-active, &.fade-leave-active
        transition all 0.5s
      &.fade-enter, &.fade-leave-active
        opacity 0
        background rgba(7, 17, 27, 0)
      .detail-wrapper
        width: 100%
        min-height 100%
        .detail-main
          margin-top 64px
          padding-bottom 64px
          .name
            line-height 16px
            font-size 16px
            text-align: center
            font-weight 700
          .star-wrapper
            margin-top 16px
            padding 2px 0
            text-align center
          .supports
            width: 80%
            margin: 0 auto
            .supports-item
              padding 0 12px
              margin-bottom 12px
              font-size 0
              &:last-child
                margin-bottom 0
              .iconMap
                width: 16px
                height: 16px
                vertical-align: top
                margin-right 6px
              .text
                line-height 16px
                font-size 12px
          .bulletin
            width 80%
            margin 0 auto
            padding 0 12px
            font-size 12px
            line-height 2em
      .detail-close
        width 32px
        height 32px
        margin -64px auto 0
        clear both
        font-size 32px
      .test
        position fixed
</style>
