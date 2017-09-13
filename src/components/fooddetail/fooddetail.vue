<template>
  <div class="food" v-show="showFood" ref="food">
    <div class="image-header">
      <img class="image" :src="targetFood.image">
      <div class="back"><i class="back-icon" @click="hide()">&lt;</i></div>
    </div>
    <div class="content">
      <h1 class="title">{{targetFood.name}}</h1>
      <div class="sale-detail"> 
        <span class="sold-count">月售{{targetFood.sellCount}}份</span>
        <span class="rating">好评率{{targetFood.rating}}%</span>
      </div>
      <div class="price">
        <span class="cur-price">￥{{targetFood.price}}</span>
        <span class="old-price" v-show="targetFood.oldPrice">{{targetFood.oldPrice}}</span>
      </div>
      <div class="add-cart">
        <div class="add" v-show="showButton" @click="addFirst($event)"><span>加入购物车</span></div>
        <div class="cartcontrol-wrapper" v-show="!showButton">
          <cartcontrol :food="targetFood"></cartcontrol>
        </div>
      </div>
    </div>
    <split v-show="targetFood.info"></split>
    <div class="introduction" v-show="targetFood.info">
      <h1 class="title">商品介绍</h1>
      <div class="info">{{targetFood.info}}</div>
    </div>
    <split></split>
    <div class="ratings">
      <h1 class="title">商品评价</h1>
      <ratingcontrol :onlyContent="onlyContent" @toggle="toggleContent" :ratings="targetFood.ratings" :ratingChoose="ratingChoose" @switch="switchRating"></ratingcontrol>
      <div class="ratings-wrapper">
        <ul class="rating-list" v-show="targetFood.ratings && targetFood.ratings.length">
          <li class="raitng-item" v-show="showItem(item.rateType, item.text)" v-for="item in targetFood.ratings">
            <span class="time">{{item.rateTime | timeFilter}}</span>
            <div class="user-info"> 
              <span class="name">{{item.username}}</span>
              <img class="avatar" :src="item.avatar" width="12" height="12"></span>
            </div>
            <div class="content">
              <span class="text">{{item.text}}</span>
            </div>
          </li>
        </ul>
        <div class="no-ratings" v-show="!targetFood.ratings || !targetFood.ratings.length">
          <span class="no-rating-text">暂无评价</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import cartcontrol from '../cartcontrol/cartcontrol';
  import split from '../split/split';
  import ratingcontrol from '../ratingcontrol/ratingcontrol';
  import BScroll from 'better-scroll';
  import Vue from 'vue';
  import {formatDate} from '../../common/js/date.js';

  const ALL = 2;
  export default {
    data () {
      return {
        showFood: false,
        onlyContent: true,
        ratingChoose: ALL
      };
    },
    props: {
      targetFood: {
        type: Object
      }
    },
    components: {
      cartcontrol,
      split,
      ratingcontrol
    },
    methods: {
      hide () {
        this.showFood = false;
      },
      show () {
        this.showFood = true;
        this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
      },
      addFirst (event) {
        if (!event._constructed) {
          return;
        }
        Vue.set(this.targetFood, 'count', 1);
      },
      toggleContent () {
        this.onlyContent = !this.onlyContent;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      switchRating (type) {
        this.ratingChoose = type;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      showItem (type, text) {
        if (type === this.ratingChoose || this.ratingChoose === ALL) {
          if ((this.onlyContent && text) || !this.onlyContent) {
            return true;
          }
        }
        return false;
      }
    },
    computed: {
      showButton () {
        if (this.targetFood.count > 0) {
          return false;
        } else {
          return true;
        }
      }
    },
    filters: {
      timeFilter: function (time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh-mm');
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  *
    margin: 0
    padding: 0
  .food
    position: fixed
    left: 0
    top: 0
    bottom: 48px
    z-index: 30
    width: 100%
    background: #fff
    font-family: 'Microsoft YaHei', 'PingFang SC', 'STHeitiSC-Light', 'Helvetica-Light', arial, sans-serif
    .image-header
      position: relative
      width: 100%
      height: 0
      padding-bottom: 100%
      .image
         position: absolute
         top: 0
         left: 0
         width: 100%
         height: 100% 
      .back
        position: absolute
        top: 10px
        left: 0
        .back-icon
          display: block
          font-size: 14px
          font-weight: 700
          color: #fff
    .content
      position: relative
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
        .add
          position: absolute
          right: 18px
          bottom: 18px
          width: 74px
          height: 24px
          line-height: 24px
          border-radius: 12px
          background: rgb(0,140,220)
          text-align: center
          font-size: 10px
          color: #fff
        .cartcontrol-wrapper
          position: absolute
          right: 18px
          bottom: 18px
    .introduction
      width: 100%
      padding: 18px
      .title
        margin-bottom: 6px
        line-height: 14px
        font-size: 14px
        font-weight: 400
        color: rgb(7,17,27)
      .info
        padding: 0 8px 0 8px
        line-height: 24px
        font-size: 12px
        font-weight: 200
        color: rgb(77,66,93)
    .ratings
      padding: 18px 18px 18px 0
      .title
        margin-bottom: 6px
        line-height: 14px
        padding-left: 18px
        font-size: 14px
        font-weight: 400
        color: rgb(7,17,27)
      .ratings-wrapper
        border-top: 1px solid rgba(7,17,27,0.1)
        .rating-list
          padding: 0 18px 0 18px
          .raitng-item
            position: relative
            border-bottom: 1px solid rgba(7,17,27,0.1)
            padding-top: 16px
            list-style: none
            font-size: 0
            .time
              display: inline-block
              line-height: 12px
              font-size: 10px
              color: rgb(147,153,159) 
            .user-info
              position: absolute
              right: 0
              top: 16px
              font-size: 0
              .name
                display: inline-block
                margin-right: 6px
                line-height: 12px
                font-size: 10px
                color: rgb(147,153,159) 
              .avatar
                border-radius: 50%
            .content
              line-height: 20px
              font-size: 12px
              color: rgb(147,153,159)   
      .no-ratings
        padding: 16px 0
        font-size: 12px
        color: rgba(77,85,93,0.2)
</style>