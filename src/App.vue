<template>
<div >
  <v-header :seller="seller"></v-header>
  <div class="tab border-1px">
    <router-link to="/goods" class="tab-item">商品</router-link>
    <router-link to="/ratings" class="tab-item">评价</router-link>
    <router-link to="/seller" class="tab-item">商家</router-link>
  </div>
  <keep-alive><router-view :seller="seller"></router-view></keep-alive>
</div>
</template>

<script type="text/ecmascript-6">
  import header from 'components/header/header.vue'
  import {urlParse} from 'common/js/util.js'
  const ERR_OK = 0
  export default {
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse()
            return queryParam.id
          })()
        }
      }
    },
    created() {
      this.$http.get('/api/seller?id=' + this.seller.id).then((response) => {
        response = response.body
        if (response.errno === ERR_OK) {
          // 给seller扩展属性 id
          this.seller = Object.assign({}, this.seller, response.data)
        }
      })
    },
    components: {
      'v-header': header
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "common/stylus/mixin.styl"
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    border-1px(rgba(7, 17, 27, 0.1))
    .tab-item
      flex: 1
      display: block
      text-align: center
      font-size: 14px
      color: rgb(77, 85, 93)
      &.active
        color: rgb(240, 20, 20)
</style>
