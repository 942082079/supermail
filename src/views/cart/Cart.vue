<template>
  <div class="cart">
    <!-- 导航 -->
    <nav-bar class="nav-bar">
      <div slot="center">购物车({{cartLength}})</div>
    </nav-bar>
    <!-- 商品列表 -->
    <scroll class="wrapper" ref="scroll">
      <div class="content">
        <cart-list/>
      </div>
    </scroll>
    <!-- 底部汇总 -->
    <cart-bottom-bar/>
  </div>
</template>

<script>
  import {mapGetters} from 'vuex'

  import NavBar from '@/components/common/navbar/NavBar'
  import Scroll from '@/components/common/scroll/Scroll'

  import CartList from './childComps/CartList'
  import CartBottomBar from './childComps/CartBottomBar'

  export default {
    components: {
      NavBar,
      CartList,
      Scroll,
      CartBottomBar
    },
    computed: {
      ...mapGetters(['cartLength'])
    },
    activated() {
      this.$refs.scroll.scroll.refresh()
    }
  }
</script>

<style scoped>
  .nav-bar {
    background-color: var(--color-tint);
    color: #fff
  }
  .cart {
    height: 100vh;
  }

  .wrapper {
    height: calc(100% - 99px);
    overflow: hidden;
  }
</style>