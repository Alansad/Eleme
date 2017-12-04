<template>
  <div class="cartcontrol">
    <transition name="move"><div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="descart"><i class="icon iconfont icon-jian inner"></i></div></transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add" @click.stop.prevent="addcart"><i class="icon iconfont icon-jia1"></i></div>
  </div>
</template>

<script>
    import Vue from 'vue'
    export default {
      props:{
        food:{
          type:Object
        }
      },
      methods:{
        addcart(event) {
          if(!event._constructed){
           return
          }
          if(!this.food.count){
            Vue.set(this.food,'count',1)
          }else{
            this.food.count++
          }
        },
        descart(event) {
          if(!event._constructed){
            return
          }
          this.food.count--
        }
      }
    }
</script>

<style>
  .cartcontrol{
    font-size:0;
  }
  .cartcontrol .cart-decrease {
    display:inline-block;
    vertical-align: top;
    padding:6px;
  }
  .cartcontrol .move-enter-active, .move-leave-active {
    transition: all 0.4s linear;
  }
  .cartcontrol .move-enter, .move-leave-to{
    opacity:0;
    transform:translateX(10px) rotate(180deg);
  }
  .cartcontrol .cart-count{
    display: inline-block;
    vertical-align: top;
    width:12px;
    padding-top:6px;
    line-height:24px;
    text-align: center;
    font-size:10px;
    color:rgb(147,153,159);
  }
  .cartcontrol .cart-add{
    display: inline-block;
    vertical-align: top;
    padding:6px;
    line-height: 24px;
    color:rgb(0,160,220);
  }
  .cartcontrol .cart-decrease .iconfont{
    display:inline-block;
    font-size:24px;
    line-height: 24px;
    color:rgb(0,160,220);
  }
  .cartcontrol .cart-add .iconfont{
    font-size: 25px;
  }
</style>
