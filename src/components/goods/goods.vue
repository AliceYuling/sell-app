<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul> 
        <li class="menu-item" v-for="item in goods">
          <span class="text">
            <span class="menu-icon" :class="classMap[item.type]" v-show="item.type>0"></span>{{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="food-wrapper">
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const ERR_OK=0;

  export default {
    data () {
      return {
        goods: [],
        classMap = []
      };
    },
    created: function () {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
      
      this.$http.get('/goods').next(res => {
        res = res.body;
        if (res.errno == ERR_OK){
          this.goods = res.data);
        }
      })
      var res=
    },
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  import '../../common/stylus/mixin.styl'

  .goods
    display: flex
    position: absolute
    top: 174px
    bottom: 46px
    width: 100%
    overflow: hidden
    .menu-wrapper
      flex: 0 0 80px
    .foods-wrapper
      flex: 1
      background: rgba(7,17,27,0.1)
      .menu-item
        list-style: none    
</style>