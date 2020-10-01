<template>
  <div class="">
    <swiper class="detail-swiper">
      <swiper-item v-for="item in topImages" :key="item.index">
        <img :src="item" alt="">
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
  import Swiper from '@/components/common/swiper/Swiper'
  import SwiperItem from '@/components/common/swiper/SwiperItem'

  export default {
    components: {
      Swiper,
      SwiperItem 
    },
    props: {
      topImages: {
        type: Array,
        default() {
          return []
        }
      }
    },
    methods: {
    },
  }
</script>

<style scoped>
  .detail-swiper {
    height: 400px;
    overflow: hidden;
  }
</style>