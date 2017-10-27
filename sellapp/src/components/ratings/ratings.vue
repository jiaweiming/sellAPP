<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <span class="star1"></span>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <span class="star1"></span>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">平均送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingsselect @select="selectRating" @toggle="toggleContent" :selectType="selectType" :onlyContent="onlyContent"
                    :ratings="ratings"></ratingsselect>
      <div class="rating-wrapper">
        <ul>
          <li v-for="rating in ratings" v-show="needShow(rating.rateType, rating.text)" class="rating-item">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length">
                <span class="icon-thumb_up"></span>
                <span class="item" v-for="item in rating.recommend">{{item}}</span>
              </div>
              <div class="time">
                {{rating.rateTime | formatDate}}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import split from '../split/split.vue'
  import ratingsselect from '../ratingsselect/ratingsselect.vue'
  import {formatDate} from '../../../static/js/date.js'

  const ALL = 2  // 所有评价
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        ratings: [],
        selectType: ALL,
        onlyContent: true,
        desc: {}
      }
    },
    methods: {  // 判断需要展示的条件，全部展示还是好评、差评
      needShow (type, text) {
        if (this.onlyContent && !text) {
          return false
        }
        if (this.selectType === ALL) {
          return true
        } else {
          return type === this.selectType
        }
      },
      selectRating (type) {
        this.selectType = type
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      },
      toggleContent () {  // 只看内容，评价页
        this.onlyContent = !this.onlyContent
        this.$nextTick(() => {
          this.scroll.refresh()
        })
      }
    },
    created () {  // 从定义的接口获取数据
      this.$http.get('/api/ratings').then((response) => {
        response = response.body
        if (response.errno === 0) { // 0表示成功
          this.ratings = response.data
          this.$nextTick(() => {
            this.scroll = new BScroll(this.$refs.ratings, {
              click: true
            })
          })
        }
      })
    },
    filters: {  // 将买家评价的时间通过正则转成标准时间格式
      formatDate (time) {
        let date = new Date(time)
        return formatDate(date, 'yyyy-MM-dd hh:mm')
      }
    },
    components: {
      split,
      ratingsselect
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../../static/css/mixin.styl"
  .ratings
    position:absolute;top:174px;bottom:0;left:0;width:100%;overflow:hidden;
    .overview
      display:flex;padding:18px 0;
      .overview-left
        flex:0 0 137px;width:137px;border-right:1px solid rgba(7,17,27,.1);text-align:center;padding:6px 0;
        @media only screen and (max-width:320px)
          flex:0 0 120px;width:120px;
        .score
          line-height:28px;font-size:24px;color:rgb(255,153,0);margin-bottom:6px;
        .title
          line-height:12px;font-size:14px;color:rgb(7,17,27);margin-bottom:8px;
        .rank
          line-height:10px;font-size 10px;color:rgb(147,153,159);
      .overview-right
        flex:1;padding-left:24px;
        .score-wrapper
          margin-bottom:8px;line-height:16px;
          .title,.score
            display inline-block;font-size:12px;vertical-align:top;line-height;18px;height:20px;
            text-align:center;color:rgb(7,17,27);
          .star1
            width:100px;height:18px;display inline-block;line-height:18px;
            background-image :url("./star.jpg");background-position:100px 166px;
        .delivery-wrapper
          line-height:16px;
          .title,.delivery
            font-size:12px;color:rgb(7,17,27);
    .rating-wrapper
      padding: 0 18px
      .rating-item
        display: flex; padding: 18px 0; border-1px(rgba(7, 17, 27, 0.1));
        .avatar
          flex: 0 0 28px; width: 28px; margin-right: 12px;
          img
            border-radius: 50%
        .content
          position: relative flex: 1
          .name
            margin-bottom: 4px; line-height: 12px; font-size: 10px; color: rgb(7, 17, 27);
          .star-wrapper
            margin-bottom: 6px; font-size: 0;
            .delivery
              display: inline-block; vertical-align: top; line-height: 12px; font-size: 10px; color: rgb(147, 153, 159);
          .text
            margin-bottom: 8px; line-height: 18px; color: rgb(7, 17, 27); font-size: 12px;
          .recommend
            line-height: 16px; font-size: 0;
            .icon-thumb_up, .item
              display: inline-block; margin: 0 8px 4px 0; font-size: 9px;
            .icon-thumb_up
              color: rgb(0, 160, 220)
            .item
              padding: 0 6px; border: 1px solid rgba(7, 17, 27, 0.1); border-radius: 1px; color: rgb(147, 153, 159); background: #fff;
          .time
            position: absolute; top: 0; right: 0; line-height: 12px; font-size: 10px; color: rgb(147, 153, 159);
</style>
