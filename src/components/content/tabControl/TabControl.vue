<template>
  <div class="tab-control">
    <div v-for="(item, index) in titles" 
    :key="item" 
     class="tab-control-item" 
    :class="{active: index == currentIndex}"
    @click="itemClick(index)">
      <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    titles: Array
  },
  data() {
    return {
      currentIndex: 0
    }
  },
  methods: {
    itemClick(index) {
      this.currentIndex = index
      this.$emit('tabClick', index)
    }
  }
}
</script>

<style>
  .tab-control {
    background-color: #fff;
    display: flex;
  }
  .tab-control-item {
    flex: 1;
    height: 40px;
    line-height: 40px;
    text-align: center;
  }

  .tab-control-item span {
    padding: 5px;
  }

  .active {
    color: var(--color-high-text);
  }
  .active span{
    border-bottom: 2px solid var(--color-tint);
  }

</style>