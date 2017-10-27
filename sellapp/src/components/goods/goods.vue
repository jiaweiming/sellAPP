<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper" >
        <ul>
          <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}"
              @click="selectMenu(index,$event)">
          <span class="text border-1px">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
          </span>
          </li>
        </ul>
      </div>
    <div class="foods-wrapper" ref="foodsWrapper">
        <ul>
          <li  v-for="item in goods" class="food-list food-list-hook">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="food in item.foods" class="food-item border-1px">
                <div class="icon"><img width="57" height="57" :src="food.icon" alt=""></div>
                <div class="content">
                  <h2 class="name">{{food.name}}</h2>
                  <p class="desc">{{food.description}}</p>
                  <div class="extra">
                    <span>月售{{food.sellCount}}份</span>
                    <span>好评率{{food.rating}}%</span>
                  </div>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                  </div>
                  <div class="cartControl-wrapper">
                    <cartControl :food="food"></cartControl>
                  </div>
                </div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    <shopCart ref="shopCart" :select-foods="selectFoods" :deliveryPrice="seller.deliveryPrice"
                :minPrice="seller.minPrice"></shopCart>
  </div>
</template>
<script type="text/ecmascript-6">
  import BScroll from 'better-scroll'
  import shopCart from '../shopCart/shopCart.vue'
  import cartControl from '../cartControl/cartControl.vue'
  export default {
    props: { // 接收seller 父组件传来
      seller: {
        type: Object
      }
    },
    data () { // 存放各种需要的数据array，对象等
      return {
        goods: [],
        listHeight: [],
        scrollY: 0,
        selectedFood: {}
      }
    },
    computed: {
      currentIndex () {  // 计算菜单的高度，对应跳转到具体的菜品
        for (let i = 0; i < this.listHeight.length; i++) {
          let height1 = this.listHeight[i]
          let height2 = this.listHeight[i + 1]
          if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
            return i
          }
        }
        return 0
      },
      selectFoods () { // 遍历food数组中的数据，获得后存入数组备用
        let foods = []
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food)
            }
          })
        })
        return foods
      }
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      this.$http.get('/api/goods').then((response) => { // ajax请求json数据
        response = response.body
        if (response.errno === 0) { // 0表示成功，挂载到data上备用
          this.goods = response.data
          this.$nextTick(() => {
            this._initScroll()
            this._calculateHeight()
          })
        }
      })
    },
    methods: {
      selectMenu (index, event) {
        if (!event._constructed) {
          return false
        }
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
        let ele = foodList[index]  // 定义数组的索引，从而控制跳到菜品，200ms
        this.foodsScroll.scrollToElement(ele, 200)
      },
      _initScroll () {  // 滚动组件的引用，左边菜单和右边菜品都需要上下滚动
        this.meunScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        })
        this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
          click: true,
          probeType: 3
        })  // 计算滚动菜品时对应的绝对值，从而控制左边菜单的高亮
        this.foodsScroll.on('scroll', (pos) => {
          this.scrollY = Math.abs(Math.round(pos.y))
        })
      },
      // 计算高度，遍历右边菜品的列表li,并存入数组
      _calculateHeight () {
        let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
        let height = 0
        this.listHeight.push(height)
        for (let i = 0; i < foodList.length; i++) {
          let item = foodList[i]
          height += item.clientHeight
          this.listHeight.push(height)
        }
      }
    },
    components: {
      shopCart,
      cartControl
    }
  }
</script>
<style lang="stylus" rel="stylesheet/stylus">
  @import "../../../static/css/mixin.styl"
  .goods
    display:flex;position:absolute;width:100%;overflow:hidden;top:174px;bottom:46px;
    .menu-wrapper
      flex:0 0 80px;width:80px;background: #f3f5f7;
      .menu-item
        display:table;height:54px;width:56px;line-height:14px;padding:0 12px;
        &.current
          position: relative;z-index: 10 ;margin-top: -1px; background:white; font-weight: 700
          .text
            border-none()
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
          display:table-cell;vertical-align:middle;font-size:12px;border-1px(rgba(7,17,27,.2))
    .foods-wrapper
      flex:1;
      .title
        padding-left:14px;height:26px;line-height:26px;border-left:2px solid #d9dde1;font-size:12px;
        color:rgb(147,153,159);background:#f3f5f7;
      .food-item
        display:flex;margin:18px;border-1px(rgba(7,17,27,.2));padding-bottom:18px;
        &:last-child
          border-none();margin-bottom:0;
        .icon
          flex:0 0 57px;margin-right:10px;
        .content
          flex:1;
          .name
            margin:2px 0 8px 0;height:14px;line-height:14px;font-size:14px;color:rgb(7,17,27)
          .desc, .extra
           line-height:10px;font-size:10px;color:rgb(147,153,159)
          .desc
              margin-bottom:8px;line-height:12px;
          .extra
            &count
              line-height:10px;
          .price
            font-weight:700;line-height:24px;
            .now
              color:rgb(240,20,20); margin-right:8px;font-size:14px;
            .old
             color:rgb(147,153,159);text-decoration:line-through;font-size:10px;

          .cartControl-wrapper
            position:absolute;right:0;top:0;
</style>
