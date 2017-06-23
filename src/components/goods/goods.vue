<template>
  <div class="foods">
    <div class="menu-wrapper">
      <ul class="foodstype-list">
        <li v-for="item in goods" class="foodlistli">
          <span class="text">
            <span v-if="item.type> 0" :class="shujus[item.type]" class="listlispan"></span>
            <span>{{item.name}}</span>
          </span>
        </li>
        <!--item代表每个被便历的数组元素-->
        <!--上面的v-if也可以换成v-show="...> 0"-->
      </ul>
    </div>
    <div class="foods-wrapper">
      <ul>
        <li v-for="item in goods" class="rightList">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="detailed" @click="fooddetailShow(food)">   <!--//!-->
                <div class="icon">
                  <img :src="food.icon" alt="" width="57" height="57">
                </div>
                <div class="content">
                  <h2 class="content-h2">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span>月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}%</span>
                  </div>
                  <span class="now">￥{{food.price}}</span><span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
                </div>
                <div class="cartcontrol-box">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcart :deliveryprice="seller.deliveryPrice" :minprice="seller.minPrice" :selectFoods="selectFoods"></shopcart>
    <food :food="selectfood" ref="food"></food>   <!--写在li里面有未知的bug，food是数组迭代的替代，不止在数组里可用-->
  </div>
</template>
<script type="text/ecmascript-6">
  import shopcart from '../shopcart/shopcart.vue'
  import cartcontrol from '../cartcontrol/cartcontrol.vue'
  import food from '../food/food.vue'
  export default{
    props: ['seller'],
    data () {
      return {
        goods: [],
        fooddetail: false,
        selectfood: {}
      }
    },
    created () {
      this.$http.get('/api/goods').then((response) => {
        response = response.body
        this.goods = response.data
        console.log(this.goods)
      })
      this.shujus = ['decrease', 'discount', 'guarantee', 'invoice', 'special']
    },
    components: {
      shopcart,
      cartcontrol,
      food
    },
    computed: {
      selectFoods() {
        let foods = []
        this.goods.forEach((god) => {
          god.foods.forEach((food) => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    methods: {
      fooddetailShow(food) {
        this.selectfood = food
        this.$refs.food.Show()   /* 父组件调用子组件的方法 */
      }
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../common/stylus/tubiao.styl"
  .foods
    display flex
    width 100%
    position absolute    //使购物车总是定在下面，上面的foods再多也只能在中间滚动
    top 174px
    bottom 46px
    .menu-wrapper
      flex 0 0 80px
      width 80px
      overflow auto
      &::-webkit-scrollbar
        display none
      .foodstype-list
        .foodlistli
          display table
          height 54px
          width 56px
          line-height 14px
          border-bottom 1px solid rgba(7,17,27,0.2)
          margin 0 12px
          .text
            display table-cell
            width 56px
            vertical-align middle
            line-height 14px
            font-size 12px
            .listlispan
              display inline-block
              width 12px
              height 12px
              margin-right 2px
              background-size 12px 12px
              background-repeat no-repeat
              vertical-align top
            .decrease
              bg-image("decrease_3")
            .discount
              bg-image("discount_3")
            .guarantee
              bg-image("guarantee_3")
            .invoice
              bg-image("invoice_3")
            .special
              bg-image("special_3")
      background-color #f3f5f7
    .foods-wrapper
      flex-grow 1
      overflow auto
      &::-webkit-scrollbar
        display none
      .rightList
        .title
          font-size 12px
          color rgb(147,153,157)
          background-color #f3f5f7
          line-height 26px
          padding-left 6px
          border-left 2px solid #d9dde1
        .detailed               //与title平级
          margin 18px
          padding-bottom 18px
          display flex
          border-bottom 1px solid rgba(7,17,27,0.1)
          position relative
          &:last-child
            margin-bottom 0
            border-bottom 0
          .cartcontrol-box
            position absolute
            right 0
            bottom 12px
          .content
            flex 1
            .content-h2
              font-size 14px
              line-height 14px
              margin 2px 0 8px 0
            .desc,.extra
              font-size 10px
              color rgb(147,153,159)
              line-height 16px
            .extra
              margin 8px 0
            .now
              font-size 15px
              color rgb(240,20,20)
              font-weight 700
              line-height 24px
            .old
              font-size 10px
              color rgb(147,153,159)
              line-height 24px
              text-decoration line-through
          .icon
            margin-right 10px
</style>
