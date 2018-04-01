<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click.stop.prevent="addCart"></div>
  </div>
</template>

<script type="text/ecmascript-6">
import Vue from "vue"
import eventHub from "@/components/event/eventHub"

export default {
  name: 'cartcontrol',
  data () {
    return {
      msg: ''
    }
  },
  props: {
      food: {
          type: Object
      }
  },
  methods: {
    addCart(event) {
      if(!event._constructed){ //web端防止多次触发
        return;
      }
      // console.log("addCart");
      if(!this.food.count) {
        // this.food.count = 1;
        Vue.set(this.food,"count",1);
      } else {
        this.food.count++;
      }
      eventHub.$emit('cart.add', event.target);
    },
    decreaseCart(event) {
      if(!event._constructed){
        return;
      }
      if(this.food.count){
        this.food.count--;
      }
    }
  },
  created() {
    //   console.log(this.food);
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="stylus" rel="stylesheet/stylus" scoped>
.cartcontrol
  font-size: 0
  .cart-decrease
    display: inline-block
    padding: 6px
    transition: all 0.4s linear
    &.move-enter-to, &.move-leave
      // opacity: 1
      transform: translate3d(0, 0, 0)
    .inner
      line-height: 24px
      font-size: 24px
      color: rgb(0, 160, 220)
    &.move-enter, &.move-leave-to
      opacity: 0
    &.move-enter-active, &.move-leave-active
      transform: translate3d(24px, 0, 0)
  .cart-count
    display: inline-block
    vertical-align: top
    width: 12px
    padding-top: 6px
    line-height: 24px
    text-align: center 
    font-size: 18px
    color: rgb(147, 153, 159)
  .cart-add
    display: inline-block
    padding: 6px
    line-height: 24px
    font-size: 24px
    color: rgb(0, 160, 220)
</style>
