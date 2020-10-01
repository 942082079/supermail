<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="!isActive">
      <slot name="item-icon"></slot>
    </div>
    <div v-else>
      <slot name="item-active-icon"></slot>
    </div>
    <div :style="activeStyle">
      <slot name="item-text"></slot>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      path: String,
      activeColor: {
        type: String,
        default: '#d4237a'
      }
    },
    computed: {
      isActive() {
        return this.$route.path.indexOf(this.path) !== -1
      },
      activeStyle() {
        return this.isActive? {color: this.activeColor} : {} 
      }
    },
    methods: {
      itemClick() {
        this.$route.path == this.path? null: this.$router.push(this.path)
      }
    }
  };
</script>

<style>
  .tab-bar-item {
    flex: 1;
    text-align: center;
    height: 55px;
    padding-top: 5px;
  }

  .tab-bar-item img {
    width: 24px;
    height: 24px;
  }

</style>