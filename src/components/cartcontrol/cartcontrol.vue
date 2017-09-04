<template>
  <div class="cart-control">
    <div class="cart-decrease" v-show="food.count>0" @click="cartDecrease($event)">-</div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-increase" @click="cartIncrease($event)">+</div>
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
      },
      cartDecrease (event) {
        if (!event._constructed) {
          return false;
        }
        this.food.count -= 1;
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
      border: 2px solid rgb(0,160,220)
      border-radius: 50%
      background: #fff
      text-align: center
      font-size: 24px
      font-weight: 700
      color: rgb(0,160,220)
    .cart-count
      display: inline-block
      width: 24px
      height: 30px
      line-height: 30px
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
      border: 2px solid rgb(0,160,220)
      border-radius: 50%
      background: rgb(0,160,220)
      text-align: center
      font-size: 24px
      font-weight: 700
      color: #fff
</style>