<template>
  <div class="goods">
    <!-- 菜单列 -->
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text border-1px"><span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>{{item.name}}</span>
        </li>
      </ul>
    </div>
    <!-- 食物商品列 -->
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <!-- 商品列表 -->
        <li class="food-list" v-for="item in goods">
          <!-- 标题 -->
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <!-- 食物列表 -->
            <li class="food-item border-1px" v-for="food in item.foods">
              <!-- 图标 -->
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <!-- 内容 -->
              <div class="content">
                <!-- 食物名称 -->
                <h2 class="name">{{food.name}}</h2>
                <!-- 食物描述 -->
                <p class="desc">{{food.description}}</p>
                <!-- 额外的描述 -->
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率{{food.rating}}%</span>
                </div>
                <!-- 售价 -->
                <div class="price">
                  <span class="new">￥{{food.price}}</span>
                  <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from "better-scroll"

const ERR_OK = 0;

export default {
  name: 'goods',
  data () {
    return {
      msg: 'I am goods',
      goods: []
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  created() {
    var self = this;
    self.classMap = ['decrease','discount','guarantee','invoice','special'];
    self.axios.get('/api/goods').then(function(response){
      // console.log(response);
      let res = response.data;
      if(res.errno == ERR_OK){
        self.goods = res.data;
        // console.log(self.goods)
        self.$nextTick(() => {
          self._initScroll();
        });
      }
    }).catch(function(error){
      console.log(error);
    });
  },
  methods: {
    _initScroll() {
      // console.log(this.$refs)
      this.menuScroll = new BScroll(this.$refs.menuWrapper,{});
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{});
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
@import '../../common/stylus/mixin.styl'

.goods
  display: flex
  position: absolute 
  top: 174px
  bottom: 46px
  width: 100%
  overflow: hidden 
  .menu-wrapper
    flex: 0 0 80px
    width: 80px
    background: #f3f5f7
    .menu-item
      display: table //使用table布局
      width: 56px
      height: 54px
      padding: 0 12px
      line-height: 24px
      font-weight: 200
      .icon
        display: inline-block
        width: 16px
        height: 16px
        vertical-align: top
        margin-right: 6px
        background-size: 16px 16px
        background-repeat: no-repeat
        &.decrease
          bg-image('decrease_3')
        &.discount
          bg-image('discount_3')
        &.guarantee
          bg-image('guarantee_3')
        &.invoice
          bg-image('invoice_3')
        &.special
          bg-image('special_3')
      .text
        display: table-cell
        width: 56px
        vertical-align: middle
        border-1px(rgba(7, 17, 27, 0.1))
        font-size: 12px
  .foods-wrapper
    flex: 1
    .title
      padding-left: 14px
      height: 26px
      line-height: 26px
      border-left: 2px solid #d9dde1
      background-color: #f3f5f7
      font-size: 12px
      color: rgb(147, 153, 159)
    .food-item
      display: flex 
      margin: 18px
      padding-bottom: 18px
      border-1px(rgba(7, 17, 27, 0.1))
      &:last-child
        margin-bottom: 0
        border-none()
      .icon
        flex: 0 0 57px
        margin-right: 10px
      .content
        flex: 1
        .name
          margin: 2px 0 8px 0
          height: 14px
          line-height: 14px
          font-size: 14px
          color: rgb(7, 17, 27)
        .desc, .extra
          line-height: 10px
          font-size: 10px
          color: rgb(147, 153, 159)
        .desc
          margin-bottom: 8px
        .extra
          .count
            margin-right: 12px
        .price
          line-height: 24px
          font-weight: 700
          .new
            margin-right: 8px
            font-size: 14px
            color: rgb(240, 20, 20)
          .old
            text-decoration: line-through
            font-size: 10px
            color: rgb(147, 153, 159)
</style>
