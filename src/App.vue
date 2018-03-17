<template>
  <div id="app">
    <vheader :seller="seller"></vheader>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <!-- 展示tab -->
    <router-view></router-view>
    <!-- <div class="content">
      I am content
    </div> -->
  </div>
</template>

<script>
import vheader from '@/components/header/vheader'

const ERR_OK = 0;

export default {
  name: 'App',
  data() {
    return {
      seller: {}
    };
  },
  created() {
    var self = this;
    self.axios.get('/api/seller').then(function(response){
      // console.log(response);
      let res = response.data;
      if(res.errno == ERR_OK){
        self.seller = res.data;
        // console.log(self.seller)
      }
    }).catch(function(error){
      console.log(error);
    });
  },
  components: {
    vheader
  }
}
</script>

<style  lang="stylus" rel="stylesheet/stylus" scoped>
@import './common/stylus/mixin.styl';

#app
  .tab
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    // border-bottom: 1px solid rgba(7,17,27,0.1)
    border-1px(rgba(7,17,27,0.1))
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77,85,93)
        &.active
          color: rgb(240,20,20)
</style>
