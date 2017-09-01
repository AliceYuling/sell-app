<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i class="icon_keyboard_arrow_right"></i>
      </div>
    </div>
    <div class="bulletin-wrapper">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i class="icon-keyboard_arrow_right" @click="showDetail"></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>

    <div class="detail" v-show="detailShow">
      <div class="detail-wrapper clearfix">
      	<div class="detail-main">
      	  <h1 class="name">{{seller.name}}</h1>
      	</div>
      </div>
      <div class="detail-close" @click="hideDetail">
        <i class="icon-close">&times;</i>
      </div>
    </div>
  </div>
</template>

<script>
  export default{
    props: {
      seller: {
        type: Object
      }
    },
    data () {
      return {
        classMap: [],
        detailShow: false
      };
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods: {
      showDetail () {
        this.detailShow = true;
      },
      hideDetail () {
        this.detailShow = false;
      }
    },
    components: {
      name: 'header'
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  // @import '../../common/stylus/icon.styl'
  .header
    position: relative
    overflow: hidden
    color: #fff
    .content-wrapper
      position: relative
      padding: 24px 12px 18px 24px
      font-size: 0
      .avatar
        display: inline-block
        vertical-align: top
        img
          border-radius: 2px
      .content
        display: inline-block
        margin-left: 16px
        font-family: arial
        .title
          margin: 2px 0 8px 0
          .brand
            display: inline-block
            vertical-align: top
            width: 30px
            height: 18px
            bg-image('brand')
            background-size: 30px 18px
          .name
            margin-left: 6px
            font-size: 16px
            font-weight: bold
            line-height: 18px
        .description
          margin-bottom: 10px
          line-height: 12px
          font-size: 12px
          font-weight: 200
        .support
          .icon
            display: inline-block
            vertical-align: top
            width: 12px 
            height: 12px
            margin-right: 4px
            background-size: 12px 12px
            background-repeat: no-repeat
            &.decrease
              bg-image('decrease_1')
            &.discount
              bg-image('discount_1')
            &.invoice_1
              bg-image('invoice_1')
            &.special
              bg-image('special_1')
            &.guarantee
              bg-image('guarantee_1')
          .text
            line-height: 12px
            font-size: 10px
            font-weight: 200 
      .support-count
        position: absolute
        right: 12px
        bottom: 18px
        padding: 0 8px 
        height: 24px
        line-height: 24px
        border-radius: 7px
        background: rgba(0,0,0,0.2)
        .count
          vertical-align: top
          font-size: 10px
          font-weight: 200
        .icon_keyboard_arrow_right
          margin-left: 2px
          line-height: 24px
          font-size: 10px
          right: 12px
          top: 8px
    .bulletin-wrapper
      position: relative  
      height: 28px
      line-height: 28px
      padding: 0 22px 0 12px
      overflow: hidden
      white-space: nowrap
      text-overflow: ellipsis
      // font-size: 0
      background: rgba(7,17,27,0.2)
      .bulletin-title
        display: inline-block
        vertical-align: top
        margin-top: 8px
        width: 22px
        height: 12px
        bg-image('bulletin')
        background-repeat: no-repeat
        background-size: 22px 12px
      .bulletin-text
        vertical-align: top
        line-height: 28px
        margin: 0 4px
        font-size: 10px
        font-weight: 200
      .icon-keyboard_arrow_right
        position: absolute
        font-size: 10px
        right: 12px
        top: 8px
    .background
      position: absolute
      left: 0
      top: 0
      width: 100%
      height: 100%
      z-index: -1
      filter: blur(10px)
    
    .detail
      position: fixed
      top: 0
      left: 0
      width: 100%
      height: 100%
      z-index: 100
      overflow: auto
      background: rgba(7,17,27,0.8)
      backdrop-filter: blur(10px)
      opacity: 1
      .detail-wrapper
         min-height: 100%
         width: 100%
         .detail-main
          margin-top: 64px
          padding-bottom: 64px 
          .name
          	line-height: 16px
          	text-align: center
          	font-size: 16px
          	font-weight: 700
      &.clearfix
        display: inline-block
        &: after
          display: inline-block
          content: "."
          height: 0
          line-height: 0
          clear: both
          visibility: hidden
      .detail-close
        position: relative
        width: 32px
        height: 32px
        margin: -64px auto 0 auto
        clear: both
        font-size: 32px 
        background: #000
        border-radius: 50%
        .icon-close
          display: inline-block
          margin: 0 4px      
</style>