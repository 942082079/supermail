<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <tab-control :titles="['流行', '新款', '精选']" 
      @tabClick="tabClick" 
      ref="tabControl1"
      class="tab-control"
      v-show="isTabFixed"/>
    <scroll class="wrapper" ref="scroll" 
      :probe-type="3"
      :pullUpLoad="true" 
      @scroll="contentScroll"
      @loadMore="loadMore">
      <div class="content">
        <home-swiper :banners="banners" @swiperImgLoad="swiperImageLoad"/>
        <recommend-view :recommends="recommends" />
        <feature-view />
        <tab-control :titles="['流行', '新款', '精选']" 
          @tabClick="tabClick" 
          ref="tabControl2"/>
        <goods-list :goods="goods[currentType].list" />
      </div>
    </scroll>
    <back-top @click.native="backClick" v-show="isBackTopShow"/>
  </div>
</template>

<script>
  import HomeSwiper from './childComps/HomeSwiper'
  import RecommendView from './childComps/RecommendView'
  import FeatureView from './childComps/FeatureView'

  import NavBar from '@/components/common/navbar/NavBar'
  import Scroll from '@/components/common/scroll/Scroll'

  import GoodsList from '@/components/content/goods/GoodsList'
  import TabControl from '@/components/content/tabControl/TabControl'

  import {backTopMixin} from '@/common/mixin'

  import {
    getHomeMultidata,
    getHomeGoods
  } from '@/network/home'

  export default {
    components: {
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl,
      GoodsList,
      Scroll,
    },
    mixins: [backTopMixin],
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          pop: {
            page: 0,
            list: []
          },
          new: {
            page: 0,
            list: []
          },
          sell: {
            page: 0,
            list: []
          },
        },
        currentType: 'pop',
        tabOffsetTop: 0,
        isTabFixed: false
      }
    },
    created() {
      getHomeMultidata().then(res => {
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        }),
      this.getHomeGoods('pop')
      this.getHomeGoods('sell')
      this.getHomeGoods('new')
    },
    mounted() {      
      // 防抖/节流
      let timer = null
      this.$bus.$on('homeLoadImg', () => {
          if(timer) clearTimeout(timer)
          timer = setTimeout(() => {
            console.log('$bus的refresh()')
            this.$refs.scroll.scroll.refresh && this.$refs.scroll.scroll.refresh()
          }, 100)
      })
    },
    methods: {
      /**
       * 事件监听相关方法
       */
      swiperImageLoad() {
        // offsetTop要等全部元素高度加载完成再计算
        this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop;
        this.$refs.scroll.scroll.refresh && this.$refs.scroll.scroll.refresh()
      },
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
            break;
          case 1:
            this.currentType = 'new'
            break;
          case 2:
            this.currentType = 'sell'
            break;
        }
        this.$refs.tabControl1.currentIndex = index
        this.$refs.tabControl2.currentIndex = index
      },
      contentScroll(position) {
        this.isBackTopShow = -position.y > 1000
        this.isTabFixed = -position.y > this.tabOffsetTop
      },
      loadMore() {
        this.getHomeGoods(this.currentType)
      },
      
      /** 
       * 网络请求相关方法
       */
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list)
          this.goods[type].page++
          this.$refs.scroll.scroll.finishPullUp()
        })
      }
    }
  }
</script>

<style scoped>
  #home {
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;
  }

  .wrapper {
    overflow: hidden;
    position: absolute;
    left: 0;
    right: 0;
    top: 44px;
    bottom: 49px;
  }

  .tab-control {
    position: relative;
    z-index: 9;
  }
</style>