<template>
  <div class="goods">
    <!-- 菜单列 -->
    <div class="menu-wrapper" ref="menuWrapper">
      <ul>
        <li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex === index}" @click="selectIndex(index)">
          <span class="text border-1px"><span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>{{item.name}}</span>
        </li>
      </ul>
    </div>
    <!-- 食物商品列 -->
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <!-- 商品列表 -->
        <li class="food-list food-list-hook" v-for="item in goods">
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
                <!-- 增加、减少按钮 -->
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <!-- 购物车 -->
    <shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
  </div>
</template>

<script type="text/ecmascript-6">
import BScroll from "better-scroll"
import shopcart from "@/components/shopcart/shopcart"
import cartcontrol from "@/components/cartcontrol/cartcontrol"
import eventHub from "@/components/event/eventHub"

const ERR_OK = 0;

export default {
  name: 'goods',
  data () {
    return {
      msg: 'I am goods',
      goods: [],
      listHeight: [],
      scrollY: 0,
      selectedFood: {}
    }
  },
  props: {
    seller: {
      type: Object
    }
  },
  computed: {
    // 菜单栏和商品食物栏联动效果
    currentIndex() {
      for(let i = 0;i< this.listHeight.length; i++){
        let height1 = this.listHeight[i];
        let height2 = this.listHeight[i + 1];
        if(!height2 || (this.scrollY >= height1 && this.scrollY < height2)){
          return i;
        }
      }
      return 0;
    },
    // 购物车选中的商品数量
    selectFoods() {
      let foods = [];
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            foods.push(food);
          }
        });
      });
      return foods;
    }
  },
  created() {
    var self = this;
    // 图标
    self.classMap = ['decrease','discount','guarantee','invoice','special'];
    self.axios.get('/api/goods').then(function(response){
      // console.log(response);
      let res = response.data;
      if(res.errno == ERR_OK){
        self.goods = res.data;
        // console.log(self.goods)
        // DOM渲染完成之后执行
        self.$nextTick(() => {
          self._initScroll();
          self._calculateHeight();
        });
      }
    }).catch(function(error){
      console.log(error);
    });
  },
  mounted() {
    var self = this;
    self.$nextTick(() => {
        eventHub.$on('cart.add',function(target){ 
          // console.log(target)
          self._drop(target);
        });
      });
  },
  methods: {
    // 菜单栏选中的索引
    selectIndex(index) {
      // console.log(index);
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
      let el = foodList[index];
      this.foodsScroll.scrollToElement(el, 300);
    },
    _initScroll() {
      // console.log(this.$refs)
      this.menuScroll = new BScroll(this.$refs.menuWrapper,{
        click: true
      });
      
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper,{
        click: true,
        probeType: 3
      });

      this.foodsScroll.on("scroll", (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y));
      })
    },
    // 计算商品食物栏的高度,并存入数组中
    _calculateHeight() {
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook');
      let height = 0;
      this.listHeight.push(height);
      for(let i=0;i<foodList.length;i++){
        let item = foodList[i];
        height += item.clientHeight;
        this.listHeight.push(height);
      }
      console.log(this.listHeight)
    },
    // 小球掉落
    _drop(target) {
      // console.log("_drop")
      // 体验优化
      this.$nextTick(() => {
        this.$refs.shopcart.drop(target);
      });
    }
  },
  components: {
    shopcart,
    cartcontrol
  }
  // events: {
  //   'cart.add'(target) {
  //     console.log(target)
  //     this._drop(target);
  //   }
  // }
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
      &.current
        position: relative
        z-index: 10
        margin-top: -1px
        background: #fff
        font-weight: 700
        color: rgb(240, 20, 20)
        .text
          border-none()
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
        .cartcontrol-wrapper
          position: absolute
          right: 0
          bottom: 12px
</style>
