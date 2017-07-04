<template>
  <div class="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
          <div class="score-wrapper"></div>
        </div>
      </div>
      <split></split>
      <ratingselect :ratings="ratings" :selectType="selectType" :onlyContent="onlyContent" @listevent="showlistchildren"></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="rating in ratings" class="ratingitem" v-show="needShow(rating.rateType,rating.text)">
            <div class="avatar">
              <img :src="rating.avatar" alt="" width="28px" height="28px">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">
                {{rating.deliveryTime}}
              </span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend &&rating.recommend.length">
                <span class="icon-thumb_up"></span>
                <span class="item" v-for="item in rating.recommend" >{{item}}</span>
              </div>
              <div class="time">
                {{rating.rateTime}}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star.vue'
  import split from '../split/spilt.vue'
  import ratingselect from '../ratingselect/ratingselect.vue'
  const ALL = 2

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        ratings: [],
        selectType: 0,
        onlyContent: false
      }
    },
    components: {
      star,
      split,
      ratingselect
    },
    created () {
      this.$http.get('/api/ratings').then((response) => {
        response = response.body
        this.ratings = response.data
      })
    },
    methods: {
      needShow(type, text) {
        if (this.onlyContent && !text) {
          return false
        }
        if (this.selectType === ALL) {
          return true
        } else {
          return type === this.selectType
        }
      },
      showlistchildren(index) {
        this.selectType = index
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .ratings
    position absolute
    top 174px
    bottom 0
    width 100%
    left 0
    overflow auto
    &::-webkit-scrollbar
      display none
    .overview
      display flex
      padding 18px 0
      .overview-left
        padding 6px 0
        flex 0 0 137px
        width 137px
        border-right 1px solid rgba(7,17,27,0.1)
        text-align center
        @media only screen and (max-width: 320px)
          flex 0 0 120px
          width 110px
        .score
          line-height 28px
          font-size 24px
          color rgb(255,153,0)
          margin-bottom 6px
        .title
          margin-bottom 8px
          line-height 12px
          font-size 12px
          color rgb(7,17,27)
        .rank
          line-height 10px
          font-size 10px
          color rgb(147,153,159)
          padding-bottom 6px
      .overview-right
        flex 1
        padding 6px 0 6px 24px
        @media only screen and (max-width: 320px)         //适应小屏幕
          padding-left 5px
        .score-wrapper
          margin-bottom 8px
          font-size 0
          .title
            display inline-block
            vertical-align top
            font-size 12px
            color rgb(7,17,27)
            line-height 18px
          .star
            display inline-block
            vertical-align top
            margin 0 12px
          .score
            display inline-block
            vertical-align top
            font-size 12px
            line-height 18px
        .delivery-wrapper
          .title
            font-size 12px
            color rgb(7,17,27)
            line-height 18px
          .delivery
            font-size 12px
            color rgb(147,153,159)
            margin-left 12px
    .rating-wrapper
      padding 0 18px
      .ratingitem
        display flex
        padding 18px 0
        border-bottom rgba(1, 17, 27, 0.1)
        .avatar
          flex 0 0 28px
          width 28px
          margin-right 12px
          img
            border-radius 50%             // border-radius也可以设置在img标签上
        .content
          position relative
          flex 1
          .name
            margin-bottom 4px
            line-height 12px
            font-weight 700
            font-size 10px
            color rgb(7, 17, 27)
          .star-wrapper
            margin-bottom 6px
            font-size 0
            .star
              display inline-block
              margin-right 16px
              vertical-align top
            .delivery
              display inline-block
              vertical-align top
              font-size 10px
              line-height 12px
              color rgb(147, 153, 159)
          .text
            line-height 18px
            color rgb(7, 17, 27)
            font-size 12px
            margin-bottom 8px
          .recommend
            line-height 16px
            font-size 0
            .icon-thumb_up, .item
              display inline-block
              margin 0 8px 4px 0
              font-size 9px
            .icon-thumb_up
              color rgb(0, 160, 220)
            .item
              padding 0 6px
              border 1px solid rgba(7, 17, 27, 0.1)
              border-radius 1px
              color rgb(147, 153, 159)
              background #fffff
          .time
            position absolute
            top 0
            right 0
            line-height 12px
            font-size 10px
            color rgb(147, 153, 159)
</style>
