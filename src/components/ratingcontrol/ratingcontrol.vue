<template>
  <div class="rating-control">
    <div class="rating-select">
      <span class="rating-item all" :class="{highlight:ratingChoose == ratingType.all}" @click="switchRating(2, $event)">{{ratingDesc.all}}&nbsp;{{ratings.length}}</span>
      <span class="rating-item positive" :class="{highlight:ratingChoose == ratingType.positive}" @click="switchRating(0, $event)">{{ratingDesc.positive}}&nbsp;{{positive.length}}</span>
      <span class="rating-item negative" :class="{highlight:ratingChoose == ratingType.negative}" @click="switchRating(1, $event)">{{ratingDesc.negative}}&nbsp;{{negative.length}}</span>
    </div>
    <div class="switch" :class="{active:onlyContent}" @click="toggleContent()">
      <i class="icon-check_circle"></i>
      <span class="icon-text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const POSITIVE = 0;
  const NEGATIVE = 1;
  const ALL = 2;
  export default {
    props: {
      ratings: {
        type: Array,
        default () {
          return [];
        }
      },
      onlyContent: {
        type: Boolean,
        default: false
      },
      ratingChoose: {
        type: Number,
        default: ALL
      }
    },
    data () {
      return {
        ratingDesc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        },
        ratingType: {
          all: ALL,
          positive: POSITIVE,
          negative: NEGATIVE
        }
      };
    },
    methods: {
      switchRating (type, event) {
        if (!event._constructed) {
          return;
        }
        this.$emit('switch', type);
      },
      toggleContent () {
        if (!event._constructed) {
          return;
        }
        this.$emit('toggle');
      }
    },
    computed: {
      positive () {
        return this.ratings.filter((rating) => {
          return rating.rateType === POSITIVE;
        });
      },
      negative () {
        return this.ratings.filter((rating) => {
          return rating.rateType === NEGATIVE;
        });
      }
    }
  };
</script>


<style lang="stylus" rel="stylesheet/stylus">
  .rating-control
    width: 100%
    padding-left: 18px
    .rating-select
      padding: 12px 18px 18px 0
      border-bottom: 1px solid rgba(7,17,27,0.1)
      font-size: 0
      .rating-item
        display: inline-block
        margin-right: 8px
        padding: 8px 12px 8px 12px
        line-height: 16px
        border-radius: 2px
        font-size: 12px
        color: rgb(77,85,93)
        &.all
          background: rgba(0,160,220,0.2)
          &.highlight
            background: rgb(0,160,220)
            color: #fff
        &.positive
          background: rgba(0,160,220,0.2)
          &.highlight
            background: rgb(0,160,220)
            color: #fff
        &.negative
          background: rgba(77,85,93,0.2)
          &.highlight
            background: rgb(77,85,93)
            color: #fff
    .switch
      padding: 12px 0 12px 0
      line-height: 24px
      font-size: 0
      color: rgb(147,153,159)
      &.active
        .icon-check_circle
          color: #00c850
      .icon-check_circle
        margin-right: 4px
        vertical-align: middle
        font-size: 24px
      .icon-text
        display: inline-block
        vertical-align: middle
        font-size: 12px  
</style>