<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
import header from './components/header/header';
const ERR_OK = 0;
export default {
  data () {
    return {
      seller: {}
    };
  },
  created () {
    this.$http.get('/api/seller').then((res) => {
      res = res.body;
      if (res.errno === ERR_OK) {
        this.seller = Object.assign({}, this.seller, res.data);
      }
    });
  },
  components: {
    'v-header': header
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus"> 
  @import "./common/stylus/mixin.styl"
 .tab
    display: flex
    width: 100%
    height: 34px
    line-height: 34px
    border-1px(rgba(7,17,27,0.1))
    .tab-item
      flex: 1
      text-align: center
      &>a
        display: block
        text-decoration: none
        font-size: 14px
        font-weight: 700
        color: rgb(77,85,93)
      &>.router-link-active
        text-decoration: none
        color: rgb(240,20,20)
</style>
