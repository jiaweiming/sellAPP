<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <!--头部信息-->
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
          <span class="star"></span>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>起送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}元</span>
            </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}分钟</span>
            </div>
          </li>
        </ul>
      </div>
      <split></split>
      <!--公告活动文案-->
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper border-1px">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="support-item border-1px" v-for="(item,index) in seller.supports">
            <span class="icon" :class="classMap[seller.supports[index].type]"></span>
            <span class="text">{{seller.supports[index].description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <!--商家内场实景-->
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item" v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <!--地址-->
      <div class="info">
        <h1 class="title border-1px">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos">{{info}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  // 引入滑动组件
  import BScroll from 'better-scroll'
  import split from '../split/split.vue'

  export default {
    props: {  // 接收父组件传下来的数据
      seller: {
        type: Object
      }
    },
    created () {  // 创建数组，对应的icon小图标
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
    },
    watch: {   // 监听dom渲染
      'seller' () {
        this.$nextTick(() => {
          this._initScroll()
          this._initPics()
        })
      }
    },
    mounted () {
      this.$nextTick(() => {
        this._initScroll()
        this._initPics()
      })
    },
    methods: {      // 初始化滑动，new一个滚动实例
      _initScroll () {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      },
      _initPics () {
        if (this.seller.pics) {
          let picWidth = 120
          let margin = 6  // 定义横向图片的滚动计算总的宽度，只允许X轴
          let width = (picWidth + margin) * this.seller.pics.length - margin
          this.$refs.picList.style.width = width + 'px'
          this.$nextTick(() => {
            if (!this.picScroll) {
              this.picScroll = new BScroll(this.$refs.picWrapper, {
                scrollX: true,
                eventPassthrough: 'vertical'
              })
            } else {
              this.picScroll.refresh()
            }
          })
        }
      }
    },
    components: {
      split
    }
  }
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import "../../../static/css/mixin.styl"
  .seller
    position: absolute; top: 174px; bottom: 0; left: 0; width: 100%; overflow: hidden;
    .overview
      position: relative; padding: 18px;
      .title
        margin-bottom: 8px; line-height: 14px; color: rgb(7, 17, 27); font-size: 14px;
      .desc
        padding-bottom: 18px; border-1px(rgba(7, 17, 27, 0.1)); font-size: 0;
        .star
          display: inline-block; width 100px;height:18px; vertical-align: top;
          background-image url("./star.jpg");background-position 100px 166px;
        .text
          display: inline-block; margin-right: 12px; line-height: 18px; vertical-align: top;
          font-size: 10px; color: rgb(77, 85, 93);
      .remark
        display: flex; padding-top: 18px;
        .block
          flex: 1; text-align: center; border-right: 1px solid rgba(7, 17, 27, 0.1);
          &:last-child
            border: none
          h2
            margin-bottom: 4px; line-height: 10px; font-size: 10px; color: rgb(147, 153, 159);
          .content
            line-height: 24px; font-size: 10px; color: rgb(7, 17, 27);
            .stress
              font-size: 16px
    .bulletin
      padding: 18px 18px 0 18px
      .title
        margin-bottom: 8px; line-height: 14px; color: rgb(7, 17, 27); font-size: 14px;
      .content-wrapper
        padding: 0 12px 16px 12px; border-1px(rgba(7, 17, 27, 0.1))
        .content
          line-height: 24px; font-size: 12px; color: rgb(240, 20, 20);
      .supports
        .support-item
          padding: 16px 12px; border-1px(rgba(7, 17, 27, 0.1)); font-size: 0;
          &:last-child
            border-none()
        .icon
          display: inline-block; width: 16px; height: 16px; vertical-align: top;
          margin-right: 6px; background-size: 16px 16px; background-repeat: no-repeat;
          &.decrease
            bg-img('decrease_4')
          &.discount
            bg-img('discount_4')
          &.guarantee
            bg-img('guarantee_4')
          &.invoice
            bg-img('invoice_4')
          &.special
            bg-img('special_4')
        .text
          line-height: 16px; font-size: 12px; color: rgb(7, 17, 27);
    .pics
      padding: 18px
      .title
        margin-bottom: 12px; line-height: 14px; color: rgb(7, 17, 27); font-size: 14px;
      .pic-wrapper
        width: 100%; overflow: hidden; white-space: nowrap;
        .pic-list
          font-size: 0
          .pic-item
            display: inline-block; margin-right: 6px; width: 120px; height: 90px;
            &:last-child
              margin: 0
    .info
      padding: 18px 18px 0 18px; color: rgb(7, 17, 27);
      .title
        padding-bottom: 12px; line-height: 14px; border-1px(rgba(7, 17, 27, 0.1)); font-size: 14px;
      .info-item
        padding: 16px 12px; line-height: 16px; border-1px(rgba(7, 17, 27, 0.1)); font-size: 12px;
        &:last-child
          border-none()
</style>
