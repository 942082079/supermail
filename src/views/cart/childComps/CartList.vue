<template>
  <div class="cart-list">
    <cart-list-item v-for="item in cartList" :key="item.index" :itemInfo="item"/>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex';

  import CartListItem from './CartListItem'

  export default {
    components: {
      CartListItem
    },
    data() {
      return {

      };
    },
    computed: {
      ...mapGetters(['cartList'])
    },
  }
</script>

<style scoped>

</style>