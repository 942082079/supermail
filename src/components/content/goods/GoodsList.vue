<template>
  <div class="goods-list">
    <goods-list-item v-for="item in goods" :key="item.index" :goodsItem="item"/>
  </div>
</template>

<script>
import GoodsListItem from './GoodsListItem'

export default {
  props: {
    goods: {
      type: Array
    }
  },
  components: {
    GoodsListItem
  }
}
</script>

<style>
  .goods-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-evenly;
  }
</style>