<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" alt="" width="64" height="64">  <!--!-->
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support"><!--seller.supports先会是一个underfined，在下面取值就会报错-->
          <span class="icon" :class="shuju[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
        <div v-if="seller.supports" class="support-count" @click="showdetial">
          <span>{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
        </div>
    </div>
    <div class="bulletin-warpper" @click="showdetial">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="" width="100%" height="100%">
    </div>
    <transition name="fade">
      <div class="detail" v-show="detialshow">
        <div class="detail-wrapper clearfix">
          <div class="detail-main">
            <h1 class="name">{{seller.name}}</h1>
            <star :size="48" :score=seller.score class="xingji"></star>      <!--如果不动态绑定的话传入的是一个字符串,虽然接受了seller但是没有动态绑定就像没关系一样-->
            <div class="youhui">
              <div class="line"></div>
              <div class="text">优惠信息</div>
              <div class="line"></div>
            </div>
            <ul v-if="seller.supports" class="youhuilist">
              <li v-for="(list,index) in shuju" class="list-support">
                <!--数组与内容无关的要用index，有关的直接用item-->
                <span class="icon" :class="shuju[seller.supports[index].type]"></span><span>{{seller.supports[index].description}}</span>
              </li>
            </ul>
            <div class="youhui">
              <div class="line"></div>
              <div class="text">商家公告</div>
              <div class="line"></div>
            </div>
            <p class="bulletin">{{seller.bulletin}}</p>
          </div>
        </div>
        <div class="detail-close">
          <i class="icon-close" @click="hiddendetial"></i>
        </div>
      </div>
    </transition>
  </div>
</template>
<script type="text/ecmascript-6">
  import star from '../star/star.vue'
  export default{
    props: ['seller'],
    data () {
      return {
        detialshow: false,
        greeting: 'hello',
//        属性是在这定义的，在这写了的属性才能双向绑定
        shuju: ['decrease', 'discount', 'guarantee', 'invoice', 'special']
      }
    },
    methods: {
      showdetial () {
        this.detialshow = true
      },
      hiddendetial () {
        this.detialshow = false
      }
    },
    components: {
      star
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/tubiao.styl"
  @import "../../common/stylus/style.css"
  .header
    background-color rgba(7,17,27,0.5)     //!
    overflow hidden    //清除因为blur跑出来的阴影
    color #fff
    position relative
    .content-wrapper
      padding  24px 16px 18px 24px
      position relative
      .avatar
        display inline-block
        vertical-align top
          img
            border-radius 2px
      .content
        display inline-block
        margin-left 16px
        .title
          margin 2px 0 8px 0
          font-size 16px
          font-weight bold
          .brand
            display inline-block
            vertical-align top  //!
            height 18px
            width 30px
            bg-image("brand")  //!
            background-size 30px 18px //!
            background-repeat no-repeat
          .name
            font-size 16px
            line-height 18px
            font-weight bold
            margin-left 6px
            display inline-block
        .description
          font-size 12px
          font-weight 200
          line-height 12px
          margin-bottom 10px
        .support
          .icon
            display inline-block
            width 12px
            height 12px
            margin-right 4px
            background-size 12px 12px
            background-repeat no-repeat
            vertical-align top
            &.decrease            //!
              bg-image("decrease_1")
            &.discount
              bg-image("discount_1")
            &.guarantee
              bg-image("guarantee_1")
            &.invoice
              bg-image("invoice_1")
            &.special
              bg-image("special_1")
          .text
            font-size 10px
            font-weight 200
            line-height 12px
            vertical-align top
      .support-count
        position absolute
        right 12px
        bottom 18px
        font-size 10px
        font-weight 200
        padding 0 8px
        background-color rgba(0,0,0,0.2)
        border-radius 14px
        line-height 24px
        text-align center
        &:hover
        cursor pointer
        .icon-keyboard_arrow_right
          line-height 24px
    .bulletin-warpper
      overflow hidden
      height 28px
      font-size 10px
      font-weight 200
      line-height 28px
      padding 0 12px
      white-space nowrap      //!
      text-overflow ellipsis    //!
      position relative     //!
      background-color rgba(7,17,27,0.2)
      .bulletin-title
        display inline-block
        width 22px
        height 10px
        bg-image("bulletin")
        background-size 22px 10px
        background-repeat no-repeat
        margin-right 4px
      .icon-keyboard_arrow_right    //!
        position absolute
        right 2px
        bottom 7px
    .background
      position absolute
      top 0
      left 0
      height 100%
      width 100%
      z-index -1
      filter blur(10px)
    .fade-enter-active, .fade-leave-active
        transition: opacity .5s
    .fade-enter, .fade-leave-active
      opacity: 0
    .detail
      position fixed
      z-index 1000
      width 100%
      height 100%
      background-color rgba(7,17,27,0.8)
      top 0
      left 0
      .detail-wrapper
        height 90%
        overflow auto
        .detail-main
          width 80%
          margin 0 auto
          padding-top 64px
          .name
            font-size 16px
            font-weight 700
            text-align center
          .xingji
            text-align center
            margin-top 16px
          .bulletin
            font-size 12px
            line-height 24px
            font-weight 200
            margin 0 12px
          .youhui
            display flex
            width 100%
            margin 28px auto 24px auto
            .line
              flex 1
              position relative
              border-bottom  1px solid rgba(255,255,255,0.2)
              bottom 6px
            .text
              margin 0 12px
              font-weight 700
          .youhuilist
            margin-left 12px
            .list-support
              margin-top 12px
              .icon
                font-size 12px
                font-weight 200
                vertical-align bottom
                display inline-block
                margin-right 6px
              .decrease
                display inline-block
                width 16px
                height 16px
                background-size 16px 16px
                background-repeat no-repeat
                bg-image("decrease_1")
              .discount
                display inline-block
                width 16px
                height 16px
                background-size 16px 16px
                background-repeat no-repeat
                bg-image("discount_1")
              .guarantee
                display inline-block
                width 16px
                height 16px
                background-size 16px 16px
                background-repeat no-repeat
                bg-image("guarantee_1")
              .invoice
                display inline-block
                width 16px
                height 16px
                background-size 16px 16px
                background-repeat no-repeat
                bg-image("invoice_1")
              .special
                display inline-block
                width 16px
                height 16px
                background-size 16px 16px
                background-repeat no-repeat
                bg-image("special_1")
      .detail-close
        height 10%
        font-size 32px
        width 32px
        margin 0 auto
</style>
