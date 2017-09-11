<template>
  <div class="ratings">
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
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import split from '../split/split';
  import star from '../star/star';
  export default{
    props: {
      seller: {
        Type: Object
      }
    },
    components: {
      split,
      star
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
      padding: 18px
      .overall-rating
        flex: 0 0 137px
        padding-bottom: 6px
        width: 137px
        border-right: 1px solid rgba(7,17,27,0.1)
        text-align: center
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
        padding-right: 24px
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
</style>