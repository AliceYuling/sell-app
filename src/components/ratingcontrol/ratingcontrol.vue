<template>
  <div class="rating-control">
    <div class="rating-select">
      <span class="all" :class="{highlight:ratingChoose == 0}">全部</span>
      <span class="recommend" :class="{highlight:ratingChoose == 0}">推荐</span>
      <span class="bad" :class="{highlight:ratingChoose == 0}">吐槽</span>
    </div>
    <div class="switch">
      <span class="icon-choose-content"></span>
      <span class="icon-text">只看有内容的评价</span>
    </div>
  </div>
</template>

<script type="ecmascript-6">
  export default {
    data () {
      return {
        ratings: {
          type: Array,
          default() {
            return [];
          }
        }
      }
    },
    computed: {
      ratingChoose () {
       return 0;
      }
    }
  };
</script>


<style lang="stylus" rel="stylesheet/stylus">
  .rating-control
    width: 100%
</style>