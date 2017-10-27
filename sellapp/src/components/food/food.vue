<template>
  <div v-show="showFlag" class="food"></div>
</template>
<script type="text/ecmascript-6">
  export default {
    props: {
      food: {
        type: Object
      }
    },
    data () {
      return {
        showFlag: true
      }
    },
    methods: {
      show () {
        this.showFlag = true
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  .food
    position:fixed;left:0;top:0;bottom:48px;z-index:30;width:100%;background: #fff353;
</style>
