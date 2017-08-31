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
        <div v-if="seller.supports" class="support-count">
          <span class="count">{{seller.supports.length}}个</span>
          <i class="icon_keyboard_arrow_right"></i>
        </div>
      </div>
      <div class="bulletin-wrapper">
        <span class="bulletin-title"></span>
        <span class="bulletin-text">{{seller.bulletin}}</span>
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
        classMap: []
      };
    },
    created () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    components: {
      name: 'header'
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .header
    color: #fff
    background: rgba(0,0,0,0.5)
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
            margin-left: 2
            line-height: 24px
            font-size: 10px
            right: 12px
            top: 8px
      .bulletin-wrapper
        font-size: 10px
</style>