<template>
  <div class="detail">
    <detail-nav-bar ref="nav" @titleClick="titleClick"/>
    <scroll class="wrapper" ref="scroll" :probe-type="3" @scroll="contentScroll">
      <div class="content">
        <detail-swiper :top-images="topImages"/>
        <detail-base-info :goods="goods"/>
        <detail-shop-info :shop="shop"/>
        <detail-goods-info :detail-info="detailInfo" @imgLoad="imgLoad"/>
        <detail-param-info ref="params" :paramInfo="paramInfo"/>
        <detail-comment-info ref="comment" :commentInfo="commentInfo"/>
        <goods-list ref="recommend" :goods="recommends"/>
      </div>
    </scroll>
    <back-top @click.native="backClick" v-show="isBackTopShow"/>
    <detail-bottom-bar @addToCart="addToCart"/>
  </div>
</template>

<script>
  import DetailNavBar from '@/views/detail/childComps/DetailNavBar'
  import DetailSwiper from '@/views/detail/childComps/DetailSwiper'
  import DetailBaseInfo from '@/views/detail/childComps/DetailBaseInfo'
  import DetailShopInfo from '@/views/detail/childComps/DetailShopInfo'
  import DetailGoodsInfo from '@/views/detail/childComps/DetailGoodsInfo'
  import DetailParamInfo from '@/views/detail/childComps/DetailParamInfo'
  import DetailCommentInfo from '@/views/detail/childComps/DetailCommentInfo'
  import DetailBottomBar from '@/views/detail/childComps/DetailBottomBar'

  import GoodsList from '@/components/content/goods/GoodsList'

  import Scroll from '@/components/common/scroll/Scroll'

  import {backTopMixin} from '@/common/mixin'

  import {getDetail, getRecommend, Goods, GoodsParam, Shop} from '../../network/detail'

  export default {
    name: 'Detail',
    components: {
      DetailNavBar,
      DetailSwiper,
      DetailBaseInfo,
      DetailShopInfo,
      DetailGoodsInfo,
      DetailParamInfo,
      DetailCommentInfo,
      DetailBottomBar,
      GoodsList,
      Scroll,
    },
    mixins: [backTopMixin],
    data() {
      return {
        iid: null,
        topImages: [],
        goods: {},
        shop: {},
        detailInfo: {},
        paramInfo: {},
        commentInfo: {},
        recommends: [],
        themeTopY: [0],
        currentIndex: 0,
      };
    },
    methods: {
      imgLoad() {
        this.$refs.scroll.scroll && this.$refs.scroll.scroll.refresh()
      },
      titleClick(index) {
        this.$refs.scroll.scroll.scrollTo(0, -this.themeTopY[index], 100)
      },
      contentScroll(position) {
        const positionY = -position.y
        // 让positionY和themeTopY的区间比较，修改index
        for(let i = 0; i < this.themeTopY.length - 1; i++) {
          if (this.currentIndex !== i && (positionY > this.themeTopY[i] && positionY < this.themeTopY[i+1])) {
            this.currentIndex = i
            this.$refs.nav.currentIndex = this.currentIndex
          }
        }
        this.isBackTopShow = -position.y > 1000
        this.isTabFixed = -position.y > this.tabOffsetTop
      },
      addToCart() {
        const product = {}
        product.image = this.topImages[0]
        product.title = this.goods.title
        product.desc = this.goods.desc
        product.price = this.goods.realPrice
        product.iid = this.iid
        this.$store.dispatch('addCart', product).then(res => {
          this.$toast.showTime(res)
        })
      }
    },
    created() {
      this.iid = this.$route.params.iid
      getDetail(this.iid).then(res => {
        let data = res.result
        this.topImages = data.itemInfo.topImages
        this.goods = new Goods(data.itemInfo, data.columns, data.shopInfo.services)
        this.shop = new Shop(data.shopInfo)
        this.detailInfo = data.detailInfo
        this.paramInfo = new GoodsParam(data.itemParams.info, data.itemParams.rule)
        if(data.rate.cRate !==0) {
          this.commentInfo = data.rate.list[0]
        }
      })

      getRecommend().then(res => {
        this.recommends = res.data.list
      })
    },
    mounted() {
    },
    updated() {
      this.$bus.$on('detailLoadImg', () => {
        try {
          this.$refs.scroll.scroll.refresh()
            this.themeTopY = [0]
            // $nextTick是当DOM加载完成时调用，但是此时img并不一定加载完了
            // offsetTop不对，很大可能都是图片未加载完
            this.themeTopY.push(this.$refs.params.$el.offsetTop - 44)
            this.themeTopY.push(this.$refs.comment.$el.offsetTop - 44)
            this.themeTopY.push(this.$refs.recommend.$el.offsetTop - 44)
            this.themeTopY.push(Number.MAX_VALUE)
        } catch (error) {}
      })
    },
  }
</script>

<style scoped>
  .detail {
    height: 100vh;
    position: relative;
    z-index: 9999;
    background-color: #fff;
  }

  .wrapper {
    height: calc(100vh - 102px);
    overflow: hidden;
  }
</style>