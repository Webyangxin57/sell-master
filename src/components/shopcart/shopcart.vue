<template>
  <div class="shopcart">
    <div class="content" @click="toggleList">
      <div class="content-left">
        <div class="logo-wrapper" >
          <div class="logo" :class="{hightlogo: totalCount>0}">
            <i class="icon-shopping_cart"></i>
          </div>
          <div class="num" v-show="totalCount>0">{{totalCount}}</div>
        </div>
        <div class="price" :class="{hightprice: totalCount>0}">￥{{totalPrice}}</div>
        <div class="desc">另需配送费￥{{deliveryprice}}元</div>
      </div>
      <div class="content-right"  @click.stop="pay">
        <div class="pay" :class="{payend: totalPrice>=minprice}">{{pricedesc}}</div>
      </div>
    </div>
    <transition name="fade">
      <div class="list-mask" v-show="listshow" @click="hidelist"></div>
    </transition>
    <transition name="fold">
    <div class="shopcart-list" v-show="listshow">
      <div class="list-header">
        <h1 class="title">购物车</h1>
        <span class="empty" @click="empty">清空</span>
      </div>
        <div class="list-content">
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
</template>
<script type="text/ecmascript-6">
  import cartcontrol from '../cartcontrol/cartcontrol.vue'
  export default {
    data () {
      return {
        fold: true
      }
    },
    props: {
      selectFoods: {
        type: Array,
        default () {
          return []
        }
      },
      deliveryprice: {
        type: Number,
        default: 0
      },
      minprice: {
        type: Number,
        default: 0
      }
    },
    computed: {
      totalPrice () {
        let total = 0
        this.selectFoods.forEach((food) => {
          total += food.price * food.count
        })
        return total
      },
      totalCount () {
        let count = 0
        this.selectFoods.forEach((food) => {
          count += food.count
        })
        return count
      },
      pricedesc () {
        if (this.totalPrice === 0) {
          return `还差${this.minprice}元起送`
        } else if (this.minprice > this.totalPrice > 0) {
          let diff = this.minprice - this.totalPrice
          return `还差${diff}元起送`
        } else {
          return '去结算'
        }
      },
      listshow() {
        if (!this.totalCount) {
          return false
        }
        let show = !this.fold
        return show
      }
    },
    methods: {
      toggleList() {
        if (!this.totalCount) {
          return false
        }
        this.fold = !this.fold
      },
      empty() {
        this.selectFoods.forEach((food) => {
          food.count = 0
        })
      },
      hidelist() {
        this.fold = true
      },
      pay() {
        if (this.totalPrice < this.minprice) {
          return
        }
        window.alert(`支付￥${this.totalPrice}元`)
      }
    },
    components: {
      cartcontrol
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .shopcart
    position fixed
    bottom 0
    left 0
    width 100%
    z-index 5
    height 48px
    .list-mask
      position fixed
      width 100%
      height 100%
      top 0
      right 0
      background rgba(7,17,27,0.6)
      z-index -15
      &.fade-enter,&.fade-leave-to
        opacity 0
      &.fade-enter-active,&.fade-leave-active
        transition all 1s
    .content
      background-color #141d27
      display flex
      font-size 0
      .content-left
        flex 1
        .logo-wrapper
          display inline-block
          position relative
          top -10px
          margin 0 12px
          padding 6px
          width 56px
          height 56px
          vertical-align top
          border-radius 50%
          background #141d27
          box-sizing border-box
          .logo
            width 100%
            height 100%
            border-radius 50%
            background-color #2b343c
            text-align center
            color #80858a
            &.hightlogo
              background-color deepskyblue
              color #fff
            .icon-shopping_cart
              font-size 24px
              line-height 44px
          .num
            position absolute
            top 0
            right 0
            width 24px
            height 16px
            line-height 16px
            text-align center
            border-radius 16px
            background-color red
            color white
            font-size 10px
        .price
          display inline-block
          vertical-align top
          line-height 24px
          margin-top 12px
          padding-right 12px
          border-right 1px solid rgba(255,255,255,0.4)
          color rgba(255,255,255,0.4)
          font-size 16px
          font-weight 700
          &.hightprice
            color rgb(255,255,254)
        .desc
          display inline-block
          font-size 10px
          line-height 24px
          vertical-align top
          margin 12px 0 0 12px
          color rgba(255,255,255,0.4)
    .content-right
      flex 0 0 105px
      width 105px
      text-align center
      .pay
        height 48px
        line-height 48px
        font-size 12px
        color rgba(255,255,255,0.4)
        font-weight 700
        background-color #2b343c
        &.payend
          background-color dodgerblue
          color white
    .shopcart-list
      overflow hidden
      position absolute
      left 0
      top 0
      z-index -1
      width 100%
      transform translate3d(0,-100%,0)
      &.fold-enter, &.fold-leave-to
        transform translate3d(0,0,0)
        opacity: 0
      &.fold-enter-active, &.fold-leave-active
        transition all 1s
      .list-header
        height 40px
        line-height 40px
        padding 0 18px
        background #f3f5f7
        border-bottom 1px solid rgba(7,17,27,0.1)
        .title
          float left
          font-size 14px
          color rgb(7,17,27)
        .empty
          float right
          font-size 12px
          color rgb(0,160,220)
      .list-content
        padding 0 18px
        max-height 217px
        overflow auto
        background-color #fff
        &::-webkit-scrollbar
          display none
        .food
          position relative
          padding 12px 0
          box-sizing border-box
          border-bottom 1px solid rgba(7,17,27,0.1)
          .name
            line-height 24px
            font-size 14px
            color rgb(7,17,27)
          .price
            position absolute
            right 90px
            bottom 12px
            line-height 24px
            font-size 14px
            color rgb(240,20,20)
            font-weight 700
          .cartcontrol-wrapper
            position absolute
            bottom 6px
            right 0
</style>
