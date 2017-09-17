<template>
  <div>
    <div class="goods">
      <div class="menu-wrapper" ref="menuWrapper">
        <ul> 
          <li class="menu-item .border-1px" v-for="(item,index) in goods" :class="{'current':listIndex===index}" @click="selectMenu(index,$event)">
            <span class="text">
              <span class="menu-icon" :class="classMap[item.type]" v-show="item.type>0"></span>{{item.name}}
            </span>
          </li>
        </ul>
      </div>
      <div class="food-wrapper" ref="foodWrapper">
        <ul>
          <li v-for="item in goods" class="food-list food-list-hook border-1px">
            <h1 class="title">{{item.name}}</h1>
            <ul>
              <li v-for="food in item.foods" class="food-item" @click="showFoodDetail(food, $event)">
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
                <div class="cartcontrol-wrapper">
                  <cartcontrol :food="food"></cartcontrol>
                </div> 
              </li>
            </ul>
          </li>
        </ul>
      </div>
      <foodcart ref="foodcart" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :selectedFoods="selectedFoods">
      </foodcart>
    </div>
    <fooddetail :targetFood="targetFood" ref="foodDetail">
    </fooddetail>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import foodcart from '../foodcart/foodcart';
  import cartcontrol from '../cartcontrol/cartcontrol';
  import fooddetail from '../fooddetail/fooddetail';
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
        scrollY: 0,
        listHeight: [],
        targetFood: {
          type: Object
        }
      };
    },
    components: {
      foodcart,
      cartcontrol,
      fooddetail
    },
    created: function () {
      this.classMap = ['decrease', 'discount', 'special'];
      this.$http.get('/api/goods').then((res) => {
        res = res.body;
        if (res.errno === ERR_OK) {
          this.goods = res.data;
          this.$nextTick(() => {
            this._initScroll();
            this._calHeight();
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
          click: true,
          probeType: 3
        });
        this.foodScroll.on('scroll', (pos) => {
          if (pos.y <= 0) {
            this.scrollY = Math.abs(Math.round(pos.y));
          }
        });
      },
      _calHeight () {
        var foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
        let height = 0;
        this.listHeight.push(height);
        for (let i = 0; i < foodList.length; i++) {
          let listItem = foodList[i];
          height += listItem.clientHeight;
          this.listHeight.push(height);
        }
      },
      selectMenu (index, event) {
        var foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
        if (!event._constructed) {                   // 阻止浏览器原生点击事件
          return false;
        }
        this.foodScroll.scrollToElement(foodList[index], 300);     // 滚动到相应元素位置
      },
      showFoodDetail (food, event) {
        if (!event._constructed) {
          return;
        }
        this.targetFood = food;
        this.$refs.foodDetail.show();
      }
    },
    computed: {
      listIndex () {
        for (let i = 0; i < this.listHeight.length; i++) {
          let currentHeight = this.listHeight[i];
          let nextHeight = this.listHeight[i + 1];
          if (!nextHeight || (this.scrollY >= currentHeight && this.scrollY < nextHeight)) {
            return i;
          }
          // return 0;
        }
      },
      selectedFoods () {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count > 0) {
              foods.push(food);
            }
          });
        });
        return foods;
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus" scoped>
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
        border-1px(rgba(7,17,27,0.1))
        list-style: none  
        &.current
          positon: relative
          z-index: 99
          margin-top: -1px
          background: #FFF
          font-weight: bold
          border-none()
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
          position: relative
          margin: 18px 18px 0 18px
          padding-bottom: 18px
          border-1px(rgba(7,1,27,0.1))
          &.last-child
            border-none()
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
          .cartcontrol-wrapper
            position: absolute
            right: 18px 
            bottom: 12px         
    .foodcart
      position: fixed
</style>