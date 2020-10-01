<template>
  <div class="goods-list-item" @click="itemClick">
    <img v-lazy="showImage" alt="" @load="loadImg">
    <div class="goods-info">
      <p>{{goodsItem.title}}</p>
      <span class="price">{{goodsItem.price}}</span>
      <span class="collect">
        <i></i>
        {{goodsItem.cfav}}
      </span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    goodsItem: {
      type: Object
    }
  },
  computed: {
    showImage() {
      return  this.goodsItem.img || this.goodsItem.image || this.goodsItem.show.img
    }
  },
  methods: {
    loadImg() {
      if (this.$route.name == 'Home') {
        this.$bus.$emit('homeLoadImg')
      } else if(this.$route.name == 'detail') {
        this.$bus.$emit('detailLoadImg')
      }
    },
    itemClick() {
      this.$router.push('/detail/' + this.goodsItem.iid)
    }
  }
}
</script>

<style>
  .goods-list-item {
    position: relative;
    z-index: 9;
    width: 48%;
  }
  .goods-list-item img {
    width: 100%;
    border-radius: 5px;
  }

  .goods-info {
    text-align: center;
    margin-bottom: 7px;
  }

  .goods-info p {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    margin-bottom: 3px;
  }

  .goods-info .price {
    color: var(--color-high-text);
    margin-right: 40px;
  }

  .goods-info .collect i {
    position: relative;
    top: 1px;
    display: inline-block;
    height: 14px;
    width: 14px;
    background: url("~@/assets/img/common/collect.svg") 0 0/14px 14px;
  }
</style>