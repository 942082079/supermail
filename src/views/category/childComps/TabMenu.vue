<template>
  <scroll id="tab-menu">
    <div class="menu-list">
      <div class="menu-list-item"
           :class="{active: index===currentIndex}"
           v-for="(item, index) in categories"
           :key="index"
           @click="itemClick(index)">
        <span>{{item.title}}</span>
      </div>
    </div>
  </scroll>
</template>

<script>
  import Scroll from '@/components/common/scroll/Scroll'

	export default {
		name: "TabMenu",
    components: {
		  Scroll
    },
    props: {
		  categories: Array
    },
    data() {
		  return {
		    currentIndex: 0
      }
    },
    methods: {
		  itemClick(index) {
        this.currentIndex = index
        this.$emit('selectItem', index)
      }
    }
	}
</script>

<style scoped>
  #tab-menu {
    background-color: #f6f6f6;
    height: calc(100vh - 99px);
    width: 100px;
    box-sizing: border-box;
  }

  .menu-list-item {
    width: 100px;
    height: 35px;
    line-height: 35px;
    text-align: center;
    font-size: 14px;
  }

  .menu-list-item.active {
    font-weight: 400;
    color: var(--color-high-text);
    background-color: #fff;
    border-left: 3px solid var(--color-high-text);
  }
</style>
