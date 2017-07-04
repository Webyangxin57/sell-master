<template>
  <div class="seller" ref="sellerr">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}</span>元
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}</span>分钟
            </div>
          </li>
        </ul>
        <div class="favorite">
          <span class="icon-favorite" :class="{active:favorite}" @click="toggleFavorite($event)"></span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="youhuilist">
          <li v-for="(list,index) in shuju" class="list-support">
            <!--数组与内容无关的要用index，有关的直接用item-->
            <span class="icon" :class="shuju[seller.supports[index].type]"></span><span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item"  v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90" alt="">
            </li>
          </ul>
        </div>
      </div>
      <div class="info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos">
            {{info}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star.vue'
  import split from '../split/spilt.vue'
  import BScroll from 'better-scroll'

  export default {
    data() {
      return {
        shuju: ['decrease', 'discount', 'guarantee', 'invoice', 'special'],
        favorite: false
      }
    },
    watch: {
      'seller'() {
        this.$nextTick(() => {
          this.scroll = new BScroll(this.$refs.sellerr, {})
        })
        if (!this.picScroll) {
          if (this.seller.pics) {
            this.$nextTick(() => {
              let picWidth = 120
              let margin = 6
              let width = (picWidth + margin) * this.seller.pics.length - margin
              this.$refs.picList.style.width = width + 'px'
              this.picScroll = new BScroll(this.$refs.picWrapper, {
                scrollX: true,
                eventPassthrough: 'vertical'
              })
            })
          }
        } else {
          this.picScroll.refresh()
        }
      }
    },
    mounted() {
      if (!this.$refs.sellerr) {
        return
      }
      this.$nextTick(() => {
        this.scroll = new BScroll(this.$refs.sellerr, {click: true})
      })
      if (!this.picScroll) {
        if (this.seller.pics) {
          this.$nextTick(() => {
            let picWidth = 120
            let margin = 6
            let width = (picWidth + margin) * this.seller.pics.length - margin
            this.$refs.picList.style.width = width + 'px'
            this.picScroll = new BScroll(this.$refs.picWrapper, {
              scrollX: true,
              eventPassthrough: 'vertical'
            })
          })
        }
      } else {
        this.picScroll.refresh()
      }
    },
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      star,
      split
    },
    computed: {
      favoriteText() {
        return this.favorite ? '已收藏' : '收藏'
      }
    },
    methods: {
      toggleFavorite(event) {
        if (!event._constructed) {
          return
        }
        this.favorite = !this.favorite
        console.log(1)
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/tubiao.styl"
  .seller
    position absolute
    overflow hidden
    width 100%
    top 174px
    bottom 0
    left 0
    .overview
      position relative
      padding 18px
      .title
        margin-bottom 8px
        line-height 14px
        color rgb(7,17,27)
        font-size 14px
      .desc
        padding-bottom 18px
        font-size 0
        border-bottom 1px solid rgba(7,17,27,0.1)
        .star
          display inline-block
          margin-right 8px
          vertical-align top
        .text
          margin-right 12px
          display inline-block
          vertical-align top
          color rgb(77,85,93)
          font-size 10px
          line-height 18px
      .remark
        display flex
        padding-top 18px
        .block
          flex 1
          text-align center
          border-right 1px solid rgba(7,17,27,0.1)
          &:last-child
            border-right none
          h2
            margin-bottom 4px
            line-height 10px
            font-size 10px
            color rgb(147, 153, 149)
          .content
            line-height 24px
            font-size 10px
            color rgb(7, 17, 27)
            .stress
              font-size 24px
      .favorite
        width 50px
        position absolute
        right 5px
        top 18px
        text-align center
        .icon-favorite
          display block
          margin-bottom 4px
          color #d4d6d9
          line-height 24px
          font-size 24px
          &.active
            color rgb(240,20,20)
      .text
        line-height 10px
        font-size 10px
        color rgb(77,85,93)
    .bulletin
      padding 18px 18px 0 18px
      .title
        margin-bottom 8px
        line-height 14px
        color rgb(7,17,27)
        font-size 14px
      .content-wrapper
        padding 0 12px 16px 12px
        border-bottom rgba(7,17,27,0.1)
        .content
          line-height 24px
          font-size 12px
          color rgb(240,20,20)
      .youhuilist
        .list-support
          border-bottom 1px solid rgba(7,17,27,0.1)
          padding 16px 12px
          .icon
            display inline-block
            width 16px
            height 16px
            margin-right 4px
            background-size 16px 16px
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
            line-height 16px
            font-size 12px
    .pics
      padding 18px
      .title
        margin-bottom 12px
        line-height 14px
        color rgb(7, 17, 27)
        font-size 14px
      .pic-wrapper
        width 100%
        overflow auto
        white-space nowrap /*不换行*/
        &::-webkit-scrollbar
          display none
        .pic-list
          font-size 0
          .pic-item
            display inline-block
            margin-right 6px
            width 120px
            height 90px
            &:last-child
              margin 0
    .info
      padding 18px 18px 0 18px
      color rgb(7, 17, 27)
      .title
        padding-bottom 12px
        line-height 14px
        border-bottom 1px solid rgba(7, 17, 27, 0.1)
        color rgb(7,17,27)
        font-size 14px
      .info-item
        padding 16px 12px
        line-height 16px
        border-bottom 1px solid rgba(7, 17, 27, 0.1)
        font-size 12px
        &:last-child
          border-none()
</style>
