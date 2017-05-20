<template>
  <div class="cartcontrol">
    <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
      <img src="decrease.png" width="20xp" height="20px">
    </div>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click="addCart">
      <img src="add.png" width="20xp" height="20px">
    </div>
  </div>
</template>
<style lang="stylus" rel="stylesheet/stylus">
  .cartcontrol
    font-size 0
    width 100%
    height 100%
    margin-top -70px
    margin-right 5px
    .cart-add{
      background-color cyan
      border-radius 50%
    }
    .cart-count
      line-height 24px
      text-align center
      font-size 16px
      color red
      font-weight 700
    .cart-decrease
       background-color cyan
       border-radius 50%
</style>
<script>
  import Vue from 'vue';
  export default{
    props: {
      food: {
        type: Object
      }
    },
    methods: {
      addCart() {
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1);
        } else {
          this.food.count++;
        }
      },
      decreaseCart() {
        if (this.food.count) {
          this.food.count--;
        }
      }
    },
    created() {
      console.log(this.food);
    }
  }
</script>
