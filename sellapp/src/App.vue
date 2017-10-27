<template>
  <div>
   <v-header :seller="seller"></v-header>
    <div class="list border-1px">
      <div class="list-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="list-item">
        <router-link to="/ratings">评价</router-link>
      </div>
      <div class="list-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import header from './components/head/head.vue'// 将头部商家信息部分从外部引入
export default {
    data () {
      return {
        seller: {}
      }
    },
    // ajax请求，拿到seller数据，body方法
    created () {
      this.$http.get('/api/seller').then((response) => {
        response = response.body
        if (response.errno === 0) {
          this.seller = response.data
        }
      })
    },
    components: { // 注册一个组件header，置于最顶层
      'v-header': header
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../static/css/mixin.styl"
/*导航列表*/
  .list
    width: 100%;
    height: 40px;
    line-height: 40px;
    display: flex;
    border-1px(rgba(7,17,27,.1))
    .list-item
      flex: 1;
      text-align: center;
      .list-item a
        font-size: 14px;
        color:rgb(77,85,93);
      .list-item a:hover
        color: rgb(240,20,20)
</style>
