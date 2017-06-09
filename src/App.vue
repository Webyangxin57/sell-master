<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view></router-view>
  </div>
</template>
<script type="text/ecmascript-6">
  import header from './components/header/header.vue'
  export default {
    data () {
      return {
        seller: {}
      }
    },
    created () {
      this.$http.get('/api/seller').then((response) => {
        response = response.body
        if (response.errno === 0) {
          this.seller = response.data
          console.log(this.seller)
        }
      })
    },
    components: {
      'v-header': header
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  #app
    .tab
      display:flex
      width:100%
      height 40px
      line-height:40px
      .tab-item
        flex:1
        text-align center/*一点空格都不能乱多出来！*/
        &>a
          display block
          font-size 14px
          color rgb(77,85,93)
          &.router-link-active
            color rgb(240,20,20)
</style>
