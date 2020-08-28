<template>
  <div class="recommend">
    <div v-for="item in recommends" :key="item.index" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    recommends: {
      type: Array,
      default() {
        return []
      }
    }
  }
}
</script>

<style>
  .recommend {
    display: flex;
    text-align: center;
    padding: 15px 0 25px;
    border-bottom: 8px solid #eee;
  }

  .recommend-item {
    flex: 1;
  }

  .recommend-item img {
    width: 93%;
    margin-bottom: 10px;
  }
</style>