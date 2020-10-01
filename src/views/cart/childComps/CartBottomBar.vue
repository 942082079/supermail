<template>
  <div class="bottom-bar">
    <div class="check-area">
      <check-button :isChecked="isSelectAll" class="check-all" @click.native="checkAll"/>
      <span>全选</span>
    </div>
    <div>
      合计：{{totalPrice}}
    </div>
    <div class="calc" @click="calcClick">
      去结算：{{calcLength}}
    </div>
  </div>
</template>

<script>
  import CheckButton from '@/components/content/checkButton/CheckButton'

  export default {
    components: {
      CheckButton
    },
    data() {
      return {
        
      };
    },
    computed: {
      totalPrice() {
        return '￥' + this.$store.state.cartList.filter(item => {
          return item.checked
        }).reduce((preValue, item) => {
          return preValue + item.price * item.count
        }, 0).toFixed(2)
      },
      calcLength() {
        return this.$store.state.cartList.filter(item => item.checked).length
      },
      isSelectAll() {
        if (this.$store.state.cartList.length == 0) {return false}
        return this.$store.state.cartList.every(item => item.checked === true)
      }
    },
    methods: {
      checkAll() {
        if (this.isSelectAll) {
          this.$store.state.cartList.forEach(item => item.checked = false)
        } else {
          this.$store.state.cartList.forEach(item => item.checked = true)
        }
      },
      calcClick() {
        if (this.calcLength == 0) {
          this.$toast.showTime('无选中商品', 2000)
        } else {
          this.$toast.showTime('结算成功', 2000)
        }
      }
    }
  }
</script>

<style scoped>
  .bottom-bar {
    display: flex;
    height: 40px;
    background-color: #eee;
    position: relative;
    bottom: 40px;
    line-height: 40px;
  }

  .check-area {
    margin-left: 20px;
    margin-right: 30px;
    display: flex;
    align-items: center;
  }

  .check-all {
    margin-right: 5px;
    line-height: 20px;
    width: 20px;
    height: 20px;
  }

  .calc {
    text-align: center;
    width: 100px;
    background-color: var(--color-tint);
    color: #fff;
    position: absolute;
    right: 0px;
  }
</style>