<template>
  <div class="detail-nav-bar">
    <nav-bar>
      <div slot="left" class="back" @click="backClick">
        <img src="~@/assets/img/common/back.svg" alt="">
      </div>
      <div slot="center" class="titles">
        <div v-for="(item, index) in titles" 
            :key="item" 
             class="titles-item"
            :class="{active: index === currentIndex}"
            @click="titleClick(index)">{{item}}
        </div>
      </div>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from '@/components/common/navbar/NavBar'

  export default {
    components: {
      NavBar
    },
    data() {
      return {
        titles: ['商品', '参数', '评论', '推荐'],
        currentIndex: 0
      };
    },
    methods: {
      titleClick(index) {
        this.currentIndex = index
        this.$emit('titleClick', index)
      },
      backClick() {
        this.$router.replace('/')
      }
    }
  }
</script>

<style scoped>
  .titles {
    display: flex;
  }
  
  .titles-item {
    flex: 1;
  }

  .active {
    color: var(--color-high-text)
  }

  .back img {
    margin-top: 10px;
  }

  .detail-nav-bar {
    position: relative;
    z-index: 999;
    background-color: #fff;
  }
</style>