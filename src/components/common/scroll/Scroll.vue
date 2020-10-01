<template>
  <div ref="wrapper">
    <slot></slot>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        defalut: false
      }
    },
    data() {
      return {
        scroll: null
      }
    },
    mounted() {
      this.scroll = new BScroll(this.$refs.wrapper, {
        click: true,
        probeType: this.probeType,
        pullUpLoad: this.pullUpLoad,
        useTransition: false
      })

      // 监听滚动
      if(this.probeType === 2 || this.probeType ===3) {
        this.scroll.on('scroll', (position) => {
          this.$emit('scroll', position)
        })
      }
 
      // 监听上拉加载更多
      if(this.pullUpLoad) {
        this.scroll.on('pullingUp', () => {
          this.$emit('loadMore')
        })
      }
    },
  }
</script>

<style scoped>

</style>