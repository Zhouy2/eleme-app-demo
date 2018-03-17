<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item in goods" class="menu-item">
          <span class="text border-1px"><span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>{{item.name}}</span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper"></div>
  </div>
</template>

<script type="text/ecmascript-6">
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
      }
    }).catch(function(error){
      console.log(error);
    });
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
</style>
