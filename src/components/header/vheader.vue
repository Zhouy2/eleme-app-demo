<template>
  <div class="vheader">
    <!-- 内容区域 -->
    <div class="content-wrapper">
      <!-- 头像区域 -->
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <!-- 商家标题 -->
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <!-- 商家描述 -->
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <!-- 优惠 -->
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <!-- 优惠的个数 -->
      <div v-if="seller.supports" class="support-count" @click="showDetail">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon-keyboard_arrow_right"></i>
      </div>
    </div>
    <!-- 公告 -->
    <div class="bulletin-wrapper" @click="showDetail">
      <span class="bulletion-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right"></i>
    </div>
    <!-- 背景 -->
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <!-- 详情弹层页 -->
    <transition name="fade">
    <div v-show="detailShow" class="detail">
      <!-- 详情信息 -->
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <!-- 商家名称 -->
         <h1 class="name">{{seller.name}}</h1>
         <!-- 评分 -->
         <div class="star-wrapper">
           <star :size="48" :score="seller.score"></star>
         </div>
         <!-- 优惠信息title -->
         <div class="title">
           <div class="line"></div>
           <div class="text">优惠信息</div>
           <div class="line"></div>
         </div>
         <!-- 优惠信息 -->
         <ul class="supports" v-if="seller.supports">
           <li class="support-item" v-for="item in seller.supports">
             <span class="icon" :class="classMap[item.type]"></span>
             <span class="text">{{item.description}}</span>
           </li>
         </ul>
         <!-- 商家公告title -->
         <div class="title">
           <div class="line"></div>
           <div class="text">商家公告</div>
           <div class="line"></div>
         </div>
         <!-- 商家公告信息 -->
         <div class="bulletin">
           <p class="content">{{seller.bulletin}}</p>
         </div>
        </div>
      </div>
      <!-- 关闭按钮 -->
      <div class="detail-close" @click="hideDetail">
        <i class="icon-close"></i>
      </div>
    </div>
    </transition>
  </div>
</template>

<script type="text/ecmascript-6">
import star from "../star/star"

export default {
  name: 'vheader',
  data () {
    return {
      detailShow: false
    }
  },
  methods: {
    showDetail() {
      this.detailShow = true
    },
    hideDetail() {
      this.detailShow = false
    }
  },
  props: {
    seller:{
      type: Object
    }
  },
  components: {
    star
  },
  created(){
    this.classMap = ['decrease','discount','guarantee','invoice','special']
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
  @import "../../common/stylus/mixin.styl"

  .vheader
    position: relative
    overflow: hidden
    color: #fff
    background: rgba(7,17,27,0.5)
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      font-size: 0
      .avatar
        display: inline-block
        vertical-align: top
        img
          border-radius: 2px
      .content
        display: inline-block
        margin-left: 16px
        .title
          margin: 2px 0 8px 0
          .brand
            display: inline-block
            vertical-align: top
            width: 30px
            height: 18px
            bg-image('brand')
            background-size: 30px 18px
            background-repeat: no-repeat
          .name
            margin-left: 6px
            font-weight: bold
            line-height: 18px
            font-size: 16px
        .description
            margin-bottom: 10px
            line-height: 12px
            font-size: 12px
        .support
          .icon
            display: inline-block
            vertical-align: top
            width: 12px
            height: 12px
            margin-right: 4px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.guarantee
              bg-image('guarantee_1')
            &.invoice
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
          .text
            line-height: 12px
            font-size: 10px
      .support-count
        position: absolute
        right: 12px
        bottom: 14px
        padding: 0 8px
        height: 24px
        line-height: 24px
        border-radius: 14px
        background-color: rgba(0,0,0,0.2)
        text-align: center
        .count
          vertical-align: top
          font-size: 10px
        .icon-keyboard_arrow_right
          margin-left: 2px
          line-height: 24px
          font-size: 10px
    .bulletin-wrapper
      position: relative
      height: 28px
      line-height: 28px
      padding: 0 22px 0 12px
      white-space: nowrap
      overflow: hidden
      text-overflow: ellipsis
      background-color: rgba(7,17,27,0.2)
      .bulletion-title
        display: inline-block
        vertical-align: top
        margin-top: 8px
        width: 22px
        height: 12px
        bg-image('bulletin')
        background-size: 22px 12px
        background-repeat: no-repeat
      .bulletin-text
        vertical-align: top
        margin: 0 4px
        font-size: 10px
      .icon-keyboard_arrow_right
        position: absolute
        right: 12px
        top: 8px
        font-size: 10px
    .background
      position: absolute
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: -1
      filter: blur(10px)
    .detail
      position: fixed
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: 100
      overflow: auto
      background: rgba(7,17,27,0.8)
      // backdrop-filter: blur(10px) //再iphone手机上实现详情弹框背景模糊
      &.fade-enter-active, &.fade-leave-active
        transition: opacity .8s;
      &.fade-enter,&.fade-leave-to
        opacity: 0
      .detail-wrapper
        min-height: 100%
        width: 100%
        .detail-main
          margin-top: 64px
          padding-bottom: 64px
          .name
            line-height: 16px
            text-align: center
            font-size: 16px
            font-weight: 700
          .star-wrapper
            margin-top: 16px
            padding: 2px 0
            text-align: center
          .title
            display: flex
            width: 80%
            margin: 28px auto 24px auto
            .line
              flex: 1
              position: relative 
              top: -6px
              border-bottom: 1px solid rgba(255,255,255,0.2)
            .text
              margin: 0 12px
              font-weight: 700
              font-size: 14px
          .supports
            width: 80%
            margin: 0 auto
            .support-item
              padding: 0 12px
              margin-bottom: 12px
              font-size: 0
              &.last-child
                margin-bottom: 0
              .icon
                display: inline-block
                width: 16px
                height: 16px
                vertical-align: top
                margin-right: 6px
                background-size: 16px 16px
                background-repeat: no-repeat
                &.decrease
                  bg-image('decrease_2')
                &.discount
                  bg-image('discount_2')
                &.guarantee
                  bg-image('guarantee_2')
                &.invoice
                  bg-image('invoice_2')
                &.special
                  bg-image('special_2')
              .text
                line-height: 16px
                font-weight: 200
                font-size: 12px
          .bulletin
            width: 80%
            margin: 0 auto 
            .content
              padding: 0 12px
              line-height: 24px
              font-weight: 200
              font-size: 12px
      .detail-close
        position: relative 
        width: 32px
        height: 32px
        margin: -64px auto 0 auto 
        clear: both
        font-size: 32px
</style>
