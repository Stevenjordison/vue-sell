<template>
  <div>
    <div class="shopcart">
      <div class="content">
        <div class="content-left" @click="toggleList">
          <div class="wrapper-logo">
            <div class="logo" :class="{'hightlight': totalCount>0}"><i class="icon-shopping_cart"></i></div>
            <div class="num" v-show="totalCount>0 ">{{totalCount}}</div>
          </div>
          <div class="price" :class="{'hightlight': totalPrice>0}">￥{{totalPrice}}</div>
          <div class="desc">另需配送费￥{{deliveryPrice}}</div>
        </div>
        <div class="content-right" @click="pay">
          <div class="pay" :class="payClass">{{payDesc}}</div>
        </div>
      </div>
      <div class="ball-container">
        <transition name="drop">
          <div v-for="ball in balls" v-show="ball.show">
            <div class="inner"></div>
          </div>
        </transition>
      </div>
      <transition name="bounce-up">
        <div class="shopcart-list" v-show="listShow">
          <div class="list-header">
            <h1 class="title">购物车</h1>
            <span class="empty" @click="empty">清空</span>
          </div>
          <div class="list-content" ref="listContent">
            <ul>
              <li class="food" v-for="food in selectFoods">
                <span class="name">{{food.name}}</span>
                <div class="price">
                  <span>￥{{food.price*food.count}}</span>
                </div>
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
    </div>
    <transition name="fade">
      <div class="list-mask" v-show="showfilter" @click="hidefilter"></div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
  import cartcontrol from 'components/cartcontrol/cartcontrol';
  import BScroll from 'better-scroll';

  export default{
    props: {
      selectFoods: {
        type: Array,
        default: []
      },
      deliveryPrice: {
        type: Number,
        default: 0
      },
      minPrice: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        balls: [
          {
            show: false
          }, {
            show: false
          }, {
            show: false
          }, {
            show: false
          }, {
            show: false
          }
        ],
        fold: true
      };
    },
    created() {
      this.$root.eventHub.$on('cart.add', this.drop);
    },
    computed: {
      totalPrice() {
        let total = 0;
        this.selectFoods.forEach((food) => {
          if (food.count) {
            total += food.price * food.count;
          }
        });
        return total;
      },
      totalCount() {
        let count = 0;
        this.selectFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDesc() {
        if (this.totalPrice === 0) {
          return `￥${this.minPrice}元起送`;
        } else if (this.totalPrice < this.minPrice) {
          let diff = this.minPrice - this.totalPrice;
          return `还差￥${diff}元起送`;
        } else {
          return '去结算';
        }
      },
      payClass() {
        if (this.totalPrice < this.minPrice) {
          return 'not-enough';
        } else {
          return 'enough';
        }
      },
      listShow() {
        if (!this.totalCount) {
          this.fold = true;
          console.log(this.fold);
          return false;
        }
        let show = !this.fold;
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs.listContent, {
                click: true
              });
            } else {
              this.scroll.refresh();
            }
          });
        }
        return show;
      },
      showfilter() {
        if (this.listShow && this.totalCount) {
          return true;
        }
        this.listShow = false;
        return false;
      },
      finalTotalPrice() {
        let final = this.totalPrice + this.deliveryPrice;
        return final;
      }
    },
    methods: {
//      drop(el) {
//        for (let i = 0, l = this.balls.length; i < l; i++) {
//          let ball = this.balls[i];
//          if (!ball.show) {
//            ball.show = true;
//            ball.el = el;
//            this.dropBalls.push(ball);
//            return;
//          }
//        }
//      },
      toggleList() {
        if (!this.totalCount) {
          return;
        }
        this.fold = !this.fold;
      },
      empty() {
        this.selectFoods.forEach((food) => {
          food.count = 0;
        });
      },
      hidefilter() {
        this.listShow = false;
      },
      pay() {
        if (this.totalPrice < this.minPrice) {
          return;
        }
        window.alert(`需要支付${this.finalTotalPrice}元`);
      }
    },
    components: {
      cartcontrol
    }

  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/mixin.styl"

  .shopcart {
    position fixed
    left 0
    bottom 0
    z-index: 99
    width 100%
    height 48px
    .content {
      display flex
      background #141d27
      font-size 0;
      color rgba(255, 255, 255, .4)
      .content-left {
        flex 1
        .wrapper-logo {
          display inline-block
          position relative
          vertical-align top
          top -10px
          margin-left 12px
          width 56px
          height 56px
          padding 6px
          box-sizing border-box
          border-radius 50%
          background-color #141d27
          z-index 999
          .logo {
            width 100%;
            height 100%;
            border-radius 50%;
            text-align: center;
            background-color #2b343c
            &.hightlight {
              background rgb(0, 160, 220)
              .icon-shopping_cart {
                color #fff
              }
            }
            .icon-shopping_cart {
              font-size 24px
              line-height 44px
              color #80858a
            }
          }
          .num {
            position: absolute;
            top 0
            right 0
            width 24px
            height 16px
            line-height 16px
            text-align: center
            border-radius 8px
            font-size 9px
            font-weight 700
            color #fff
            background-color rgb(240, 20, 20)
            box-shadow 0 4px 8px 0 rgba(0, 0, 0, .4)
          }
        }
        .price {
          display inline-block
          vertical-align top
          line-height 24px
          margin-top 12px
          box-sizing border-box
          padding-right 12px
          border-right 1px solid rgba(255, 255, 255, .1)
          font-size 16px
          font-weight 700
          &.hightlight {
            color #fff
          }
        }
        .desc {
          display inline-block
          vertical-align top
          line-height 24px
          margin 12px 0 0 12px
          font-size 12px
        }
      }
      .content-right {
        flex 0 0 105px
        width 105px
        .pay {
          width 100%
          height 48px
          line-height 48px
          text-align: center;
          font-size 12px
          font-weight 700
          &.not-enough {
            background #2b333b
          }
          &.enough {
            background #00b43c
            color #fff
          }
        }
      }
    }
    .ball-container {
      .ball {
        position fixed
        left 32px
        bottom 22px
        z-index 9999
        &.drop-transition {
          transition: all .4s
          .inner {
            width 16px
            height 16px
            border-radius 50%
            background-color rgb(0, 160, 220)
          }

        }
      }
    }
    .shopcart-list {
      position absolute
      top 0
      left 0
      width 100%
      background white
      transform translate3d(0, -100%, 0)
      z-index -1
      &.bounce-up-enter-active, &.bounce-up-leave-active {
        transition all 0.5s
      }
      &.bounce-up-enter, &.bounce-up-leave-active {
        transform translate3d(0, 0, 0)
      }

      .list-header {
        height 40px
        line-height 40px
        background #f3f5f7
        border-bottom 1px solid rgba(7, 17, 27, 0.1)
        .title {
          display inline-block
          font-size 14px
          font-weight 200
          color rgb(7, 17, 27)
          padding-left 18px
        }
        .empty {
          position absolute
          right 8px
          font-size 12px
          color rgb(0, 160, 220)
          padding 0 10px
        }
      }
      .list-content {
        max-height 217px
        overflow hidden
        .food {
          position relative
          display flex
          height 48px
          margin 0 18px
          border-1px(rgba(7, 17, 27, 0.1))
          .name {
            flex 1
            font-size 14px
            color rgb(7, 17, 27)
            line-height 48px
            font-weight 700
          }
          .price {
            font-size 14px
            font-weight 700
            color rgb(240, 20, 20)
            padding 0 12px 0 18px
            line-height 48px
          }
          .cartcontrol-wrapper {
            font-size 14px
            margin-top 6px
          }
        }
      }

    }

  }

  .list-mask {
    position: fixed;
    top 0
    left 0
    right 0
    bottom 0
    background rgba(7, 17, 27, 0.6)
    backdrop-filter blur(10px)
    z-index 40
    &.fade-enter-active, &.fade-leave-active {
      transition all 0.5s
    }
    &.fade-leave-active, &.fade-enter {
      opacity 0
    }
  }
</style>
