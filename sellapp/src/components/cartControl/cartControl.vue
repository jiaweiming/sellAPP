<template>
  <div class="cartControl">
    <div class="cart-decrease icon-remove_circle_outline"
         @click.stop.prevent="decreaseCart" v-show="food.count>0">
    </div>
    <div class="cart-Count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
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
    methods: {  // 此组件为加减控制，如果不是自己点击触发的事件，就直接返回
      addCart (event) {
        if (!event._constructed) {
          return
        }
        if (!this.food.count) { // 操作内部数据，点击时数量加1
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
      },
      decreaseCart (event) {
        if (!event._constructed) {
          return
        }  // 操作内部数据，点击时数量加1
        if (this.food.count) {
          this.food.count--
        }
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .cartControl
    font-size:0;
    .cart-decrease
      display:inline-block;font-size:24px;line-height :24px;padding:3px;color:rgb(0,160,220)
    .cart-Count
      display: inline-block; vertical-align: top; width: 12px; padding-top: 3px; line-height: 24px;
      text-align: center; font-size: 10px; color: rgb(147, 153, 159)
    .cart-add
      display:inline-block;font-size:24px;line-height :24px;padding:3px;color:rgb(0,160,220)
</style>
