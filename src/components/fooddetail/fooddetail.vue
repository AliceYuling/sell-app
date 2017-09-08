<template>
  <div class="food-detail" v-show="showFood" ref="foodDetail">
    <div class="image-header">
      <img class="image" :src="targetFood.image">
      <div class="back"><i class="back-icon" @click="hide()">返回</i></div>
    </div>
    <div class="content">
      <h1 class="title">{{targetFood.name}}</h1>
      <div class="sale-detail"> 
        <span class="sold-count">月售{{targetFood.sellCount}}份</span>
        <span class="rating">好评率{{targetFood.rating}}%</span>
      </div>
      <div class="price">
        <span class="cur-price">{{targetFood.price}}</span>
        <span class="old-price" v-show="targetFood.oldPrice">{{targetFood.oldPrice}}</span>
      </div>
      <div class="add-cart">
        <div class="add"></div>
        <div class="cartcontrol-wrapper">
          <cartcontrol :food="targetFood"></cartcontrol>
        </div>
      </div>
    </div>
    <div class="introduction">
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import cartcontrol from '../cartcontrol/cartcontrol';
  import BScroll from 'better-scroll';
  export default {
    data () {
      return {
        showFood: false
      };
    },
    props: {
      targetFood: {
        type: Object
      }
    },
    components: {
      cartcontrol
    },
    methods: {
      hide () {
        this.showFood = false;
      },
      show () {
        this.showFood = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.foodDetail, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food-detail
    position: fixed
    left: 0
    bottom: 48px
    width: 100%
    height: 100%
    z-index: 98
    background: #fff
    .image-header
      position: relative
      top: 0
      left: 0
      width: 100%
      height: 0
      padding-bottom: 100%
      .image
         position: absolute
         top: 0
         left: 0
         z-index: 120
         width: 100%
         height: 100% 
      .back
        position: absolute
        z-index:999
        width: 100px
        height: 100px
        top: 10px
        left: 0
        .back-icon
          display: block
          font-size: 14px
          font-weight: 700
          color: #fff
    .content
      padding: 18px
      .title
        line-height: 14px
        margin-bottom: 8px
        font-size: 14px
        font-weight: 700
        color: rgb(7,17,27)
      .sale-detail
        margin-bottom: 18px 
        font-size: 10px
        color: rgb(147,153,159)
        .sold-count
          display: inline-block
          margin-right: 12px
        .rating
          display: inline-block
      .price
        .cur-price
          line-height: 24px
          font-size: 14px
          font-weight: 700
          color: rgb(240,20,20) 
        .old-price
          line-height: 24px
          font-size: 10px
          font-weight: 700
          color: rgb(147,153,159) 
      .add-cart
        position: relative
        right: 18px
        .add
          position: absolute
          right: 0
          width: 148px
          height: 24px
          
        .cartcontrol-wrapper
          position: absolute
          right: 0
</style>