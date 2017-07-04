<template>
  <div class="cartcontrol">
    <transition name="decrease">
      <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click.stop="decreasecount($event)"></div>
    </transition>
    <div class="hezi">
      <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    </div>
    <div class="cart-add icon-add_circle" @click.stop="addcount($event)"></div>
  </div>
</template>
<script type="text/ecmascript-6">
  import Vue from 'vue'
  export default {
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addcount (event) {
        if (!event._constructed) {
          return
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
        return this.food.count
      },
      decreasecount (event) {
        if (!event._constructed) {
          return
        }
        if (this.food.count > 0) {
          this.food.count--
        }
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/style.css"
  .cartcontrol
    font-size 0
    .decrease-enter-active,.decrease-leave-active
      transition: all .5s
    .decrease-enter, .decrease-leave-to
      transform: translateX(10px) rotate(90deg)
      opacity: 0
    .hezi
      display inline-block
      width 20px
      vertical-align top
      .cart-count
        display inline-block
        font-size 10px
        line-height 24px
        height 30px
        color rgb(147,153,159)
        width 20px
        vertical-align top
        text-align center
        margin-top 2px
    .cart-add,.cart-decrease
      display inline-block
      line-height 24px
      font-size 24px
      color rgb(0,160,220)
      padding 4px
</style>
