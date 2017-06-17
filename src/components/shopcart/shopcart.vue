<template>
  <div class="shopcart">
    <div class="content">
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
      <div class="content-right">
        <div class="pay">￥{{minprice}}起送</div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
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
        return 1
      }
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
</style>
