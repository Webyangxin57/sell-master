<template>
  <div class="ratingselect">
    <div class="rating-type">
      <span class="block positive" :class="{active:count===2}" @click="select(2)">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
      <span class="block positive" :class="{active:count===0}" @click="select(0)">{{desc.positive}}<span class="count">{{tuijian.length}}</span></span>
      <span class="block negative" :class="{active:count===1}" @click="select(1)">{{desc.negative}}<span class="count">{{tucao.length}}</span></span>
    </div>
    <div class="switch" :class="{on:Content}" @click="toggleContent">
      <span class="icon-check_circle"></span>
      <span class="text">只看有内容的评价</span>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  const ALL = 2
  export default {
    data() {
      return {
        count: this.selectType,
        Content: this.onlyContent
      }
    },
    props: {
      ratings: {
        type: Array,
        default() {
          return []
        }
      },
      selectType: {
        type: Number,
        default: ALL
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      desc: {
        type: Object,
        default() {
          return {
            all: '全部',
            positive: '满意',
            negative: '不满意'
          }
        }
      }
    },
    methods: {
      select(index) {
        this.count = index
      },
      toggleContent() {
        this.Content = !this.Content
      }
    },
    computed: {                 // 计算属性不需要在data里声明
      tuijian() {
        return this.ratings.filter((rating) => {
          return rating.rateType === 0
        })
      },
      tucao() {
        return this.ratings.filter((rating) => {
          return rating.rateType === 1
        })
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .ratingselect
    .rating-type
      padding 18px 0
      margin 0 18px
      border-bottom 1px solid rgba(7,17,27,0.1)
      .block
        display inline-block
        padding 8px 12px
        margin-right 8px
        border-radius 1px
        color rgb(77,85,93)
        font-size 12px
        .count
          font-size 8px
          margin-left 2px
          line-height 16px
      .active
          color #fff
      .positive
        background rgba(0,160,220,0.2)
        &.active
          background rgb(0,160,220)
      .negative
        background rgba(77,85,93,0.2)
        &.active
          background rgb(77,85,93)
    .switch
      padding 12px 18px
      line-height 24px
      border-bottom 1px solid rgba(7,17,27,0.1)
      color rgb(147,153,159)
      &.on
        .icon-check_circle
          color #00c850
      .icon-check_circle
        font-size 24px
        margin-right 4px
        vertical-align middle
      .text
        font-size 12px
</style>
