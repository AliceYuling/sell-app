<template>
  <div class="cart-control">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="cartDecrease($event)">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
      <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
      <div class="cart-increase" @click.stop.prevent="cartIncrease($event)">
        <i class="icon-add_circle"></i>
      </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import Vue from 'vue';
  export default {
    props: {
      food: {
        type: Object,
        default: {
          count: 1
        }
      }
    },
    methods: {
      cartIncrease (event) {
        if (!event._constructed) {
          return false;
        }
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count += 1;
        }
        this.$emit('add', event.target);
      },
      cartDecrease (event) {
        if (!event._constructed) {
          return false;
        }
        this.food.count -= 1;
        // console.log(this.food);
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .cart-control
    font-size: 0
    .cart-decrease
      display: inline-block
      width: 24px
      height: 24px
      padding-bottom: 2px
      line-height: 24px
      opacity: 1
      transform: translate3d(0,0,0)
      color: rgb(0,160,220)
      .inner
        display: inline-block
        font-size: 24px
        font-weight: 700
        color: rgb(0,160,220)
        transform: rotate(0)
      &.move-enter, &.move-leave-to
        opacity: 0
        transform: translate3d(24px,0,0)
        .inner
          transform: rotate(180deg)
      &.move-enter-active, &.move-leave-active
        transition: all 0.4s linear
        .inner
          transition: all 0.4s linear
    .cart-count
      display: inline-block
      width: 24px
      height: 24px
      line-height: 24px
      vertical-align: top
      font-size: 10px
      color: rgb(147,153,159)
      font-weight: 700
      text-align: center
    .cart-increase
      display: inline-block
      width: 24px
      height: 24px
      line-height: 24px
      padding-bottom: 2px
      text-align: center
      font-size: 24px
      font-weight: 700
      color: rgb(0,160,220)
</style>