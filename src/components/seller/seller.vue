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
        <h1 class="title">公告与活动</h1>
        <div class="content">{{seller.bulletin}}</div>
        <div class="supports-wrapper">
          <ul class="supports-list">
            <li class="support-item" v-for="support in seller.supports">
              <span class="support-icon" :class="classMap[support.type]"></span>
              <span class="support-desc">{{support.description}}</span>
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="seller-pics">
        <h1 class="title">商家实景</h1>
        <div class="pics-wrapper" ref="pics">
          <ul class="pics-list" ref="picsList">
            <li class="pic-item" v-for="pic in seller.pics">
              <img :src="pic" width="120" height="90">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="seller-info">
        <h1 class="title">商家信息</h1>
        <ul class="info-list">
          <li class="info-item" v-for="info in seller.infos">
            <span class="info">{{info}}</span>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import split from '../split/split';
  import star from '../star/star';
  import BScroll from 'better-scroll';
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        classMap: []
      };
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      this.$nextTick(() => {
        this._initScroll();
        this._initPics();
      });
    },
    methods: {
      _initScroll () {
        console.log('seller:' + this.$refs.seller);
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          });
        } else {
          this.scroll.refresh();
        }
      },
      _initPics () {
        if (this.seller.pics) {
          let picWidth = 120;
          let margin = 6;
          let width = (picWidth + margin) * this.seller.pics.length - margin;
          var picsList = this.$refs.picsList;
          picsList.style.width = width + 'px';
          if (!this.picScroll) {
            this.scroll = new BScroll(this.$refs.pics, {
              scrollX: true,
              eventPassthrough: 'vertical'
            });
          } else {
            this.picScroll.refresh();
          }
        }
      }
    },
    components: {
      split,
      star
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
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
                margin-bottom: 4px
                line-height: 10px
                font-size: 10px
                color: rgb(147,153,159)
              .content
                font-size: 10px
                color: rgb(77,85,93)
                .num
                  line-height: 24px
                  font-size: 20px
                  color: rgb(7,17,27)
                  font-weight: 200
      .bulletin
        padding: 18px 18px 0 18px
        .title
          margin-bottom: 8px
          line-height: 14px
          font-size: 14px
          color: rgb(7,17,27)
        .content
          margin-bottom: 16px
          line-height: 24px
          padding-left: 12px
          padding-right: 12px
          font-size: 12px
          color: rgb(240,20,20)
          font-weight: 200
        .supports-wrapper
          .supports-list
            .support-item
              list-style: none
              padding: 16px 12px 16px 12px
              border-top: 1px solid rgba(7,17,27,0.1)
              .support-icon
                display: inline-block
                margin-right: 6px 
                width: 16px
                height: 16px
                vertical-align: top
                background-size: 16px 16px
                background-repeat: no-repeat
                &.decrease
                  bg-image('decrease_3')
                &.discount
                  bg-image('discount_3')
                &.invoice
                  bg-image('invoice_3')
                &.special
                  bg-image('special_3')
                &.guarantee
                  bg-image('guarantee_3')
              .support-desc
                display: inline-block
                line-height: 16px
                vertical-align: top
                font-size: 12px
                font-weight: 200
                color: rgb(7,17,27)
      .seller-pics
        padding: 18px 0 18px 18px
        .title
          margin-bottom: 12px
          line-height: 14px
          font-size: 14px
          color: rgb(7,17,27)
        .pics-wrapper
          width: 100%
          overflow: hidden
          white-space: nowrap
          .pics-list
            font-size: 0
            .pic-item
              display: inline-block
              margin-right: 6px
              list-style: none
              width: 120px
              height: 90px
              &:last-child
                margin-right: 0
      .seller-info
        padding: 18px 18px 0 18px
        .title
          margin-bottom: 12px
          line-height: 14px
          font-size: 14px
          color: rgb(7,17,27)
        .info-list
          .info-item
            list-style: none
            padding: 16px 12px 16px 12px
            border-top: 1px solid rgba(7,17,27,0.1)
            .info
              line-height: 16px
              font-size: 12px
              font-weight: 200
              color: rgb(7,17,27)  
</style>