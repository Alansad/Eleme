<template>
  <transition name="foodmove">
  <div v-show="showFlag" class="food" ref="food">
    <div class="food-content">
      <div class="image-header">
        <img :src="food.image">
        <div class="back" @click="hide">
          <i class="icon iconfont icon-fanhui"></i>
        </div>
      </div>
      <div class="content">
        <h1 class="title">{{food.name}}</h1>
        <div class="detail">
          <span class="sell-count">月售{{food.sellCount}}份</span>
          <span class="rating">好评率{{food.rating}}%</span>
        </div>
        <div class="price">
          <span class="now">¥{{food.price}}</span><span v-if="food.oldPrice" class="old">¥{{food.oldPrice}}</span>
        </div>
        <div class="cartcontrol-wrapper">
          <cartcontrol :food="food"></cartcontrol>
        </div>
        <transition name="shopcartfade"><div @click.stop.prevent="addFirst" class="buy" v-show="!food.count ||food.count===0">加入购物车</div></transition>
      </div>
      <split v-show="food.info"></split>
      <div class="info" v-show="food.info">
        <h1 class="title">商品信息</h1>
        <p class="text">{{food.info}}</p>
      </div>
      <split></split>
      <div class="rating">
        <h1 class="title">商品评价</h1>
        <ratingselect @select="selectRating" @toggle="toggleContent" :selectType="selectType" :onlyContent="onlyContent" :desc="desc"  :ratings="food.ratings"></ratingselect>
        <div class="rating-wrapper">
          <ul v-show="food.ratings && food.ratings.length">
            <li v-for="rating in food.ratings" class="rating-item" v-show="needShow(rating.rateType,rating.text)">
              <div class="user">
                <span class="name">{{rating.username}}</span>
                <img class="avatar" width="12px" height="12px" :src="rating.avatar">
              </div>
              <div class="time">{{rating.rateTime | formatDate}}</div>
              <p class="text">
                <span class="icon iconfont" :class="{'icon-muzhi':rating.rateType === 0,'icon-muzhizhaoxia':rating.rateType === 1}"></span>
                {{rating.text}}
              </p>
            </li>
          </ul>
          <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
        </div>
      </div>
    </div>
  </div>
  </transition>
</template>

<script>
    import BScroll from 'better-scroll';
    import cartcontrol from '../cartcontrol/cartcontrol.vue'
    import Vue from 'vue'
    import split from '../split/split.vue'
    import ratingselect from '../ratingselect/ratingselect.vue'
    import {formatDate} from '@/common/js/date'

    const POSITIVE = 0
    const NEGATIVE = 1
    const ALL = 2

    export default {
      props: {
        food: {
          type: Object
        }
      },
      data() {
        return {
          showFlag: false,
          selectType: NEGATIVE,
          onlyContent: false,
          desc: {
            all: '全部',
            positive: '推荐',
            negative: '吐槽'
          }
        }
      },
      methods: {
        show() {
          this.showFlag = true;
          this.selectType = ALL;
          this.onlyContent = true;
          this.$nextTick(() => {
            if (!this.scroll) {
              this.scroll = new BScroll(this.$refs.food, {
                click: true
              })
            } else {
              this.scroll.refresh()
            }
          })
        },
        hide() {
          this.showFlag = false;
        },
        addFirst(event) {
          if (!event._constructed) {
            return
          }
          Vue.set(this.food, 'count', 1)
        },
        selectRating(type) {
          this.selectType = type;
          this.$nextTick(() => {
            this.scroll.refresh();
          });
        },
        toggleContent() {
          this.onlyContent = !this.onlyContent;
          this.$nextTick(() => {
            this.scroll.refresh();
          });
        },
        needShow(type,text) {
          if(this.onlyContent && !text){
            return false
          }
          if(this.selectType === ALL){
            return true
          }else{
            return type === this.selectType
          }
        }
      },
      filters:{
        formatDate(time) {
          let date = new Date(time)
          return formatDate(date,'yyyy-MM-dd hh:mm')
        }
      },
      components:{
        cartcontrol,
        split,
        ratingselect
      }
    }
</script>

<style scoped>
  .food{
    position:fixed;
    left:0;
    top:0;
    bottom:48px;
    z-index:30;
    width:100%;
    background: #fff;
    transform:translate3D(0,0,0);
  }
  .foodmove-enter-active{
    transition:all 0.4s linear;
  }
  .foodmove-leave-active {
    transition:all 0.4s linear;
  }
  .foodmove-enter{
    transform:translate3D(100%,0,0);
  }
  .foodmove-leave-to{
    transform:translate3D(100%,0,0);
  }
  .image-header{
    position:relative;
    width:100%;
    height:0;
    padding-top:100%;
  }
  .image-header img{
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
  }
  .back{
    position:absolute;
    top:10px;
    left:0;
  }
  .back .icon-fanhui{
    display:block;
    padding:10px;
    font-size:20px;
    color:#fff;
  }
  .content{
    padding:18px;
    position:relative;
  }
  .content .title{
    line-height:14px;
    margin-bottom:8px;
    font-size:14px;
    font-weight:700;
    color:rgb(7,17,27);
  }
  .content .detail{
    margin-bottom:18px;
    line-height:10px;
    height:10px;
    font-size:0;
  }
  .content .detail .sell-count{
    font-size:10px;
    color:rgb(147,153,159);
    margin-right:12px;
  }
  .content .detail .rating{
    font-size:10px;
    color:rgb(147,153,159);
  }
  .content .price{
    font-weight:700;
    line-height:24px;
  }
  .content .price .now{
    margin-right:8px;
    font-size:14px;
    color:rgb(240,20,20);
  }
  .content .price .old{
    text-decoration: line-through;
    font-size:10px;
    color:rgb(147,153,159);
  }
  .food .cartcontrol-wrapper{
    position:absolute;
    right:12px;
    bottom:4px;
  }
  .food .buy{
    position:absolute;
    right:18px;
    bottom:18px;
    z-index:10;
    height:24px;
    line-height:24px;
    padding:0 12px;
    box-sizing:border-box;
    font-size:10px;
    border-radius:12px;
    color:#fff;
    background: rgb(0,160,220);
  }
  .shopcartfade-enter-active{
    transition:all 0.4s;
  }
  .shopcartfade-leave-active {
    transition:all 0.4s;
  }
  .shopcartfade-enter{
    opacity:0;
  }
  .shopcartfade-leave-to{
    opacity:0;
  }
  .info{
    padding:18px;
  }
  .info .title{
    line-height:14px;
    margin-bottom:6px;
    font-size:14px;
    color:rgb(7,17,27);
  }
  .info .text{
    line-height:24px;
    padding:0 8px;
    font-size:12px;
    color:rgb(77,85,93);
  }
  .food .rating{
    padding-top:18px;
  }
  .food .rating .title{
    line-height:14px;
    margin-left:18px;
    font-size:14px;
    color:rgb(7,17,27);
  }
  .food .rating .rating-wrapper{
    padding:0 18px;
  }
  .food .rating .rating-wrapper .rating-item{
    position:relative;
    padding:16px 0;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .food .rating .rating-wrapper .rating-item .user{
    position:absolute;
    line-height:12px;
    right:0;
    top:16px;
    font-size:0;
  }
  .food .rating .rating-wrapper .rating-item .user .name{
    display:inline-block;
    vertical-align: top;
    margin-right:6px;
    font-size:10px;
    color:rgb(147,153,159);
  }
  .food .rating .rating-wrapper .rating-item .user .avatar{
    border-radius:50%;
  }
  .food .rating .rating-wrapper .rating-item .time{
    margin-bottom:6px;
    line-height: 12px;
    font-size:10px;
    color:rgb(147,153,159);
  }
  .food .rating .rating-wrapper .rating-item .text{
    line-height:16px;
    font-size:12px;
    color:rgb(7,17,27);
  }
  .food .rating .rating-wrapper .rating-item .text .icon-muzhi, .icon-muzhizhaoxia{
    margin-right:4px;
    line-height:16px;
    font-size:24px;
  }
  .food .rating .rating-wrapper .rating-item .text .icon-muzhi{
    color:rgb(0,160,220);
  }
  .food .rating .rating-wrapper .rating-item .text .icon-muzhizhaoxia{
    color:rgb(147,153,159);
  }
  .food .rating .rating-wrapper .no-rating{
    padding:16px 0;
    font-size:12px;
    color:rgb(147,153,159);
  }
</style>
