<template>
  <div class="food-cart">
    <div class="content">
      <div class="content-left">
        <div class="icon-wrapper">
          <div class="cart-icon" :class="{'highlight':totalCount>0}">
            <span class="icon-shopping-cart" :class="{'highlight':totalCount>0}"></span>
            <div class="num-in-cart" v-show="totalCount>0">{{totalCount}}</div>
          </div>
        </div>
        <div class="cart-price">
          <span>￥{{totalPrice}}</span>
        </div>
        <div class="description">
          另需配送费￥{{deliveryPrice}}元
        </div>
      </div>
      <div class="content-right">
        <span class="pay-desc" :class="{'pay-enough':payEnough === 1, 'not-enough':payEnough===0}">{{payDescription}}</span>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {
    data () {
      return {
        balls: [
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          },
          {
            show: false
          }
        ],
        dropBalls: []
      };
    },
    props: {
      deliveryPrice: {
        type: Number
      },
      minPrice: {
        type: Number
      },
      selectedFoods: {
        type: Array,
        default () {
          return [
            {
              price: 10,
              count: 1
            }
          ];
        }
      }
    },
    computed: {
      totalPrice () {
        let total = 0;
        this.selectedFoods.forEach((food) => {
          console.log('food count:' + food.count + 'food price:' + food.price);
          total += food.count * food.price;
        });
        console.log(total);
        return total;
      },
      totalCount () {
        let count = 0;
        this.selectedFoods.forEach((food) => {
          count += food.count;
        });
        return count;
      },
      payDescription () {
        if (this.totalPrice === 0) {
          this.payEnough = 0;
          return `￥${this.minPrice}起送`;
        } else if (this.totalPrice < this.minPrice) {
          let payDiff = this.minPrice - this.totalPrice;
          this.payEnough = 0;
          return `还差￥${payDiff}起送`;
        } else {
          this.payEnough = 1;
          return '去结算';
        }
      }
    }
    /*
    methods: {
      drop (el) {
        for (let i=0; i < this.balls.length; i++) {
          let ball = this.balls[i];
          if (!ball.show) {
            ball.show = true;
            ball.el = el;
            this.dropBalls.push(ball);
            return;
          }
        }
      }
    } */
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .food-cart
    position: fixed
    left: 0
    bottom: 0
    z-index: 99
    width: 100%
    height: 48px
    .content
      display: flex
      background: #241d27
      font-size: 0
      .content-left
        flex: 1
        .icon-wrapper
          display: inline-block
          position: relative
          top: -10px
          margin: 0 12px
          padding: 6px
          width: 56px
          height: 56px
          box-sizing: border-box
          background: #241d27
          vertical-align: top
          border-radius: 50%
          .cart-icon
            width: 100%
            height: 100%
            border-radius: 50%
            background: rgba(255,255,255,0.4)
            text-align: center
            &.highlight
              background: rgb(0,160,220)
            .icon-shopping-cart
              line-height: 24px
              color: rgba(255,255,255,0.4)
              font-size: 24px
              &.highlight
                color: #fff
            .num-in-cart
              position: absolute
              width: 24px
              height: 16px
              top: 0
              right: 0
              border-radius: 6px
              line-height: 16px
              background: rgb(240,20,20)
              font-size: 9px
              font-weight: 700
              color: #fff
              box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4)
        .cart-price, .description
          display: inline-block
          margin-top: 12px
          line-height: 24px
          padding-right: 12px
          font-size: 16px
          font-weight: 700
          color: rgba(255,255,255,0.4)  
        .description
          font-size: 12px
          padding-left: 12px
          border-left: 1px solid rgba(255,255,255,0.1)
      .content-right
        flex: 0 0 105px
        width: 105px
        .pay-desc
          display: inline-block
          height: 48px
          width: 100%
          line-height: 24px
          padding:12px 8px 12px 8px
          box-sizing: border-box
          text-align: center
          font-size: 12px
          font-weight: 700
          &.pay-enough
            background: #00b43c
            color: #fff
          &.not-enough
          	background: #2b333b
          	color: rgba(255,255,255,0.4)
</style>