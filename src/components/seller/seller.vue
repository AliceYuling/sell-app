<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="seller-overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc">
          <star :size="36" :score="seller.score"></star>
          <span class="rating-count">({{seller.ratingCount}})</span>
          <span class="sell-count">月售{{seller.sellCount}}单</span>
        </div>
        <div class="delivery">
          <ul class="delivery-info">
            <li class="delivery-item">
              <h2>起送价</h2>
              <div class="content">
                <span class="num">{{seller.minPrice}}</span>元
              </div>
            </li>
            <li class="delivery-item">
              <h2>商家配送</h2>
              <div class="content">
                <span class="num">{{seller.deliveryPrice}}</span>元
              </div>
            </li>
            <li class="delivery-item">
              <h2>平均配送时间</h2>
              <div class="content">
                <span class="num">{{seller.deliveryTime}}</span>分钟
              </div>
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
      </div>
      <div class="seller-image">
      </div>
      <div class="seller-info">
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import split from '../split/split';
  import star from '../star/star';
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    components: {
      split,
      star
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  *
    margin: 0
    padding: 0
  .seller
    position: absolute
    top: 174px
    left: 0
    bottom: 0
    width: 100%
    overflow: hidden
    font-family: 'Microsoft YaHei', 'PingFang SC', 'STHeitiSC-Light', 'Helvetica-Light', arial, sans-serif
    .seller-content
      .seller-overview
        padding: 18px
        .title
          margin-bottom: 8px
          line-height: 14px
          font-size: 14px
          color: rgb(7,17,27) 
        .desc
          padding-bottom: 18px
          border-bottom: 1px solid rgba(7,17,27,0.1)
          .star
            display: inline-block
            vertical-align: middle
          .rating-count, .sell-count
            display: inline-block
            line-height: 18px
            vertical-align: middle
            font-size:10px
            color: rgb(77,85,93)
           .sell-count
             display: inline-block
             margin-left: 12px
        .delivery
          padding-top: 18px
          .delivery-info
            font-size: 0
            .delivery-item
              display: inline-block
              width: 33%
              list-style: none
              border-right: 1px solid rgba(7,17,27,0.1)
              text-align: center
              &:last-child
                border-right: none
              h2
                line-height: 10px
                font-size: 10px
                color: rgb(147,153,159)
              .content
                font-size: 10px
                color: rb(77,85,93)
                .num
                  line-height: 24px
                  font-size: 20px
                  color: rgb(7,17,27)
                  font-weight: 200 
</style>