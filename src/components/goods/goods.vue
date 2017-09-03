<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul> 
        <li class="menu-item" v-for="item in goods">
          <span class="text">
            <span class="menu-icon" :class="classMap[item.type]" v-show="item.type>0"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="food-wrapper" ref="foodWrapper">
      <ul>
        <li class="food-list" v-for="item in goods">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item">
              <div class="icon">
                <img width="57" height="57" :src="food.icon">
              </div>
              <div class="food-detail">
                <h2 class="name">{{food.name}}</h2>
                <p class="description"></p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span class="current-price">￥{{food.price}}</span><span v-show="food.oldPrice" class="old-price">￥{{food.oldPrice}}</span>
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
  import BScroll from 'better-scroll';
  const ERR_OK = 0;

  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        goods: [],
        classMap: [],
        scrollY: 0
      };
    },
    created: function () {
      this.classMap = ['decrease', 'discount', 'special'];
      this.$http.get('api/goods').then(res => {
        res = res.body;
        if (res.errno === ERR_OK) {
          this.goods = res.data;
          this.$nextTick(() => {
            this._initScroll();
          });
        }
      });
    },
    methods: {
      _initScroll () {
        this.menuScroll = new BScroll(this.$refs.menuWrapper, {
          click: true
        });
        this.foodScroll = new BScroll(this.$refs.foodWrapper, {
          click: true
        });
        this.foodScroll.on('scroll', (pos) => {
          if (pos.y <= 0) {
            this.scrollY = Math.abs(Math.round(pos.y));
          }
        });
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  *
    margin: 0
    padding: 0
  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    font-family: 'Microsoft YaHei', 'PingFang SC', 'STHeitiSC-Light', 'Helvetica-Light', arial, sans-serif
    .menu-wrapper
      flex: 0 0 80px
      width: 80px
      background: rgba(7,17,27,0.1)
      .menu-item
        display: table
        height: 54px
        width: 56px
        padding: 0 12px
        line-height: 14px
        // border: 1px solid rgba(7,17,27,0.5)
        list-style: none  
        .text
          display: table-cell
          width: 56px
          font-size: 12px
          vertical-align: middle
          .menu-icon
            display: inline-block
            width: 12px
            height: 12px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-image('decrease_3')
            &.discount
              bg-image('discount_3')
            &.special
              bg-image('special_3')
    .food-wrapper
      flex: 1
      .food-list
        list-style: none
        .title
          height: 26px
          padding-left: 12px
          border-left: 2px solid #d9dde1
          background: #f3f5f7
          line-height: 26px
          font-size: 12px
          color: rgb(147,153,159)
        .food-item
          display: flex
          margin: 18px 18px 0 18px
          border-bottom: 1px solid rgba(7,1,27,0.1)
          padding-bottom: 18px
          &.last-child
            border-bottom: none
            padding-bottom: 0
          .icon
            flex: 0 0 57px
            width: 57px
            margin-right: 10px
          .food-detail
            flex 1 1 auto
            .name
              margin: 2px 0 8px 0
              height:10px
              line-height: 10px
              color: rgb(7,17,27)
              font-size: 14px
            .description, .extra
              line-height: 10px
              font-size: 10px
              color: rgb(147,153,159)
            .extra
              color: rgb(147,153,159)
              .count
                display: inline-block
                margin-right: 12px
              .rating
                display: inline-block
            .price
              .current-price
                display: inline-block
                margin-right: 8px
                line-height: 24px
                font-size: 14px
                font-weight: 700
                color: rgb(240,20,20)
              .old-price
                display: inline-block
                line-height: 24px
                text-decoration: line-through
                font-size: 10px
                font-weight: 700
                color: rgb(147,153,159)
</style>