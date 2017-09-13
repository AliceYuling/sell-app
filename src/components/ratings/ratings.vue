<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overall-rating">
          <div class="score">{{seller.score}}</div>
          <div class="text">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}%</div>
        </div>
        <div class="star-rating">
          <div class="star-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-time">
            <span class="title">送达时间</span>
            <span class="time">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingcontrol :ratings="ratings" :onlyContent="onlyContent" :ratingChoose="ratingChoose" @switch="switchRating" @toggle="toggleContent"></ratingcontrol>
      <div class="ratings-wrapper">
        <ul class="rating-list" v-show="ratings && ratings.length">
          <li class="raitng-item" v-show="showItem(rating.rateType, rating.text)" v-for="rating in ratings">
            <span class="time">{{rating.rateTime | timeFilter}}</span>
            <div class="user-info"> 
              <span class="name">{{rating.username}}</span>
              <img class="avatar" :src="rating.avatar" width="12" height="12"></span>
            </div>
            <div class="content">
              <span class="text">{{rating.text}}</span>
            </div>
            <div class="recommend">
              <ul class="recommend-list">
                <li class="recommend-item" v-for="item in rating.recommend">
                  <span class="text">{{item}}</span>
                </li>
              </ul>
            </div>
          </li>
        </ul>
        <div class="no-ratings" v-show="!ratings || !ratings.length">
          <span class="no-rating-text">暂无评价</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import split from '../split/split';
  import star from '../star/star';
  import ratingcontrol from '../ratingcontrol/ratingcontrol';
  import {formatDate} from '../../common/js/date.js';
  import BScroll from 'better-scroll';
  const ERR_OK = 0;
  const ALL = 2;
  export default{
    props: {
      seller: {
        Type: Object
      }
    },
    data () {
      return {
        ratings: [],
        onlyContent: false,
        ratingChoose: 0
      };
    },
    components: {
      split,
      star,
      ratingcontrol
    },
    created: function () {
      this.$http.get('/api/ratings').then((res) => {
        res = res.body;
        if (res.errno === ERR_OK) {
          this.ratings = res.data;
          this.$nextTick(() => {
            this._initScroll();
          });
        }
      });
    },
    methods: {
      _initScroll () {
        this.scroll = new BScroll(this.$refs.ratings, {
          click: true
        });
      },
      switchRating (type) {
        this.ratingChoose = type;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
      toggleContent () {
        this.onlyContent = !this.onlyContent;
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
    filters: {
      timeFilter: function (time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh-mm');
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .ratings
    position: absolute
    top: 174px
    bottom: 0
    left: 0
    width: 100%
    overflow: hidden
    font-family: 'Microsoft YaHei', 'PingFang SC', 'STHeitiSC-Light', 'Helvetica-Light', arial, sans-serif
    .overview
      display: flex
      padding: 18px 0
      .overall-rating
        flex: 0 0 137px
        padding-bottom: 6px
        width: 137px
        border-right: 1px solid rgba(7,17,27,0.1)
        text-align: center
        @media only screen and (max-width:320px)
          flex: 0 0 115px
          width: 115px
        .score
          line-height: 28px
          font-size: 24px
          color: rgb(255,153,0)
          margin-bottom: 6px
        .text
          line-height: 12px
          font-size: 12px
          color: rgb(7,17,27)
          margin-bottom: 8px
        .rank
          line-height: 10px
          font-size: 10px
          color: rgb(147,153,159) 
      .star-rating
        flex: 1
        padding-left: 24px
        padding-right: 0
        @media only screen and (max-width:320px)
          flex: 1
          padding-left: 6px
          padding-right: 6px
        .star-wrapper
          margin-bottom: 8px
          font-size: 0
          .title
            display: inline-block
            line-height: 28px
            font-size: 12px
            color: rgb(255,153,0)
            margin-bottom: 6px
          .star
            display: inline-block
            margin: 0 12px
          .score
            display: inline-block
            line-height: 18px
            font-size: 12px
            color: rgb(255,153,0)
        .delivery-time
          font-size: 0
          .title
            display: inline-block
            line-height: 28px
            font-size: 12px
            color: rgb(255,153,0)
            margin-bottom: 6px
          .time
            display: inline-block
            margin-left: 12px
            line-height: 18px
            font-size: 12px
            color: rgb(147,153,159)
    .ratings-wrapper
      border-top: 1px solid rgba(7,17,27,0.1)
      .rating-list
        padding: 0 18px 0 18px
        .raitng-item
          position: relative
          border-bottom: 1px solid rgba(7,17,27,0.1)
          padding-top: 18px
          padding-bottom: 18px
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
          .recommend
            .recommend-list
              padding-left: 6px
              .recommend-item
                display: inline-block
                list-style: none
                font-size: 0
                .text
                  display: inline-block
                  margin-left: 8px
                  margin-top: 8px
                  max-width: 40px
                  height: 24px
                  line-height: 24px
                  border: 1px solid rgba(7,17,27,0.1)
                  border-radius: 2px
                  padding: 0 6px 0 6px
                  overflow: hidden
                  text-overflow: ellipsis
                  white-space: nowrap
                  font-size: 9px
                  color: rgb(147,153,159)
    .no-ratings
      padding: 16px 0
      font-size: 12px
      color: rgba(77,85,93,0.2)
</style>