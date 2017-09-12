<template>
  <div class="star" :class="starType">
    <span v-for="(itemClass,index) in itemClasses" class="star-item" :class="itemClass" key="index">
    </span>
  </div>
</template>

<script type="text/ecmascript-6">
  const LENGTH = 5;
  const STAR_ON = 'star-on';
  const STAR_HALF = 'star-half';
  const STAR_OFF = 'star-off';
  export default{
    props: {
      score: {
        type: Number
      },
      size: {
        type: Number
      }
    },
    computed: {
      itemClasses: function () {
        let result = [];
        let onCount = Math.floor(this.score);
        for (let i = 0; i < onCount; i++) {
          result.push(STAR_ON);
        }
        if (this.score - onCount >= 0.5) {
          result.push(STAR_HALF);
        }
        while (result.length < LENGTH) {
          result.push(STAR_OFF);
        }
        return result;
      },
      starType: function () {
        return 'star-' + this.size;
      }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/mixin.styl'
  .star
    font-size: 0
    .star-item
      display: inline-block
      background-repeat: no-repeat
    &.star-48
      .star-item
        height: 20px
        width: 20px
        margin-right: 22px
        background-size: 20px 20px
        &:last-child
          margin-right: 0
        &.star-on
          bg-image('star48_on')
        &.star-half
          bg-image('star48_half')
        &.star-off
          bg-image('star48_off')
    &.star-36
      .star-item
        height: 15px
        width: 15px
        margin-right: 6px
        background-size: 15px 15px
        &:last-child
          margin-right: 0
        &.star-on
          bg-image('star36_on')
        &.star-half
          bg-image('star36_half')
        &.star-off
          bg-image('star36_off')
    &.star-24
      .star-item
        height: 10px
        width: 10px
        margin-right: 3px
        background-size: 10px 10px
        &:last-child
          margin-right: 0
        &.star-on
          bg-image('star24_on')
        &.star-half
          bg-image('star24_half')
        &.star-off
          bg-image('star24_off')
</style>