<template>
  <div class="head">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" alt="头像" width="64" height="64">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span><span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span><span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span><i>></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletin-title"></span>
      <span class="bulletin-text">{{seller.bulletin}}</span>
      <i>></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" alt="" width="100%" height="100%">
    </div>
    <div v-show="detailShow" class="detail">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star"></div>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <div class="free">
            <ul v-if="seller.supports" class="supports">
              <li class="support-item border-1px" v-for="(item,index) in seller.supports">
                <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                <span class="text2">{{seller.supports[index].description}}</span>
              </li>
            </ul>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="seller-word">
            <p class="words">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hideDetail">X</div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  export default {
    props: { // 接收父组件穿来的json数据
      seller: {
        type: Object
      }
    },
    created () {  // 对应的5个icon小图标
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']  // 5个商家活动
    },
    data () {  // 控制蒙板是否显示
      return {
        detailShow: false
      }
    },
    methods: { // 控制蒙板是否显示
      showDetail () {
        this.detailShow = true
      },
      hideDetail () { // 控制蒙板是否显示
        this.detailShow = false
      }
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../../static/css/mixin.styl"
  .head
    position :relative;background-color: rgba(7,17,27,.5);overflow:hidden;
    /*第一个大模块，商家顶部信息*/
    .content-wrapper
      padding: 24px 12px 18px 24px;font-size: 0;position:relative;
      .avatar
        display: inline-block;vertical-align:top
        img
          border-radius:5px;
      .content
        display: inline-block;margin-left: 16px;font-size:14px;
        .title
          margin: 2px 0 8px 0;
          .brand
            display: inline-block;vertical-align:top;width:30px;height: 18px;bg-img('brand');
            background-size :30px 18px;background-repeat:no-repeat;
          .name
            margin-left :6px;font-size:16px;font-weight :bold;color:rgb(255,255,255);line-height:18px;
        .description
          margin-bottom:10px;line-height:12px;font-size :12px;color:rgb(255,255,255);font-weight:200;
        .support
          .icon
            display:inline-block;width:12px;height:12px;margin-right:4px;background-size:12px 12px;
            background-repeat:no-repeat;vertical-align:top;
            &.decrease
              bg-img('decrease_1')
            &.discount
              bg-img('discount_1')
            &.guarantee
              bg-img('guarantee_1')
            &.invoice
              bg-img('invoice_1')
            &.special
              bg-img('special_1')
          .text
            line-height:12px;vertical-align:top;font-size:10px;color:rgb(255,255,255)
      .support-count
        position:absolute;right:12px;bottom:18px;padding:0 8px;height:24px;text-align:center;
        line-height:24px;border-radius:14px;background-color:rgba(0,0,0,.2);color:#f1f1f1;
        .count
          font-size:10px;font-weight:200;
        i
          font-size:10px;line-height:24px;margin-left:2px;
    /*第二个模块，中间公告和描述*/
    .bulletin-wrapper
      height:28px;line-height:28px;padding:0 22px 0 12px;white-space:nowrap;overflow:hidden;
      text-overflow:ellipsis;color:#f1f1f1;position:relative;background-color :rgba(7,17,27,.3)
      .bulletin-title
        display:inline-block;width:22px;height:12px;bg-img('bulletin');vertical-align:top;
        background-size:22px 12px;background-repeat:no-repeat;margin-top:8px;
      .bulletin-text
        font-size:10px;line-height:30px;margin-left:4px;margin-right:4px;vertical-align:top;
      i
        font-size:10px;position:absolute;right:12px;
    //模糊背景
    .background
      position: absolute;top:0;left:0;width:100%;height:100%;z-index:-1;filter:blur(10px);
      /*模糊背景的定位，sticky footer布局*/
    .detail
      position:fixed;z-index:99;top:0;left:0;width :100%;height :100%;overflow:auto;background-color :rgba(7,17,27,.8)
      .detail-wrapper
        min-height:100%;width:100%;
        .detail-main
          margin-top:34px;padding-bottom:64px;
          .name
            font-size:16px;font-weight:700;color:white;line-height:20px;text-align:center;
          .star
              width 100px;height:18px;margin:20px auto;
              background-image url("css_sprites.png");background-position 100px 60px;
          .title
              display:flex;width:80%;margin:20px auto 24px auto;
            .line
              flex:1;position:relative;top:-6px;border-bottom:1px solid rgba(255,255,255,.5)
            .text
              padding:0 12px;font-size :14px;color:white;font-weight :500;
          .free
            padding-left:24px;
            .supports
              .support-item
                padding: 10px 12px; border-1px(rgba(7, 17, 27, 0.1)); font-size: 0;
              .icon
                display: inline-block; width: 16px; height: 16px; vertical-align: top;
                margin-right: 6px; background-size: 16px 16px; background-repeat: no-repeat;
                &.decrease
                  bg-img('decrease_2')
                &.discount
                  bg-img('discount_2')
                &.guarantee
                  bg-img('guarantee_2')
                &.invoice
                  bg-img('invoice_2')
                &.special
                  bg-img('special_2')
              .text2
                line-height: 16px; font-size: 12px; color: white;
          .seller-word
             padding:0 36px 36px 36px;color:white;font-size 12px;line-height 28px;
      .detail-close
        position:relative;width:20px;height:25px;margin:-64px auto -20px auto;clear:both;
        font-size:25px;color:rgba(255,255,255,.5);

</style>

