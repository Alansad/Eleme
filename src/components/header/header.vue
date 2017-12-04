<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img width="64" height="64" :src="seller.avatar">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <span class="icon" :class="classMap[seller.supports[0].type]"></span>
          <span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click = "showDetail">
        <span class="count">{{seller.supports.length}}个</span>
        <i>></i>
      </div>
    </div>
    <div class="bulletin-wrapper" @click = "showDetail">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <i>></i>
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%" >
    </div>
    <transition name="fade">
    <div v-show="detailShow" class="detail">
      <div class="detail-wrapper clearfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wrapper">
            <star :size="24" :score="seller.score"></star>
          </div>
          <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
          </div>
          <ul v-if="seller.supports" class="supports">
            <li class="support-item" v-for="(item,index) in seller.supports">
              <span class="icon" :class="classMap[seller.supports[index].type]"></span>
              <span class="text">{{seller.supports[index].description}}</span>
            </li>
          </ul>
          <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
          </div>
          <div class="bulletin">
            <p class="content">{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close" @click="hideDetail">
        <i class="icon iconfont icon-guanbi"></i>
      </div>
    </div>
    </transition>
  </div>
</template>

<script>
  import star from '../star/star.vue';
  export default {
    props:{
      seller:{
        type:Object
      }
    },
    data() {
      return{
        detailShow:false
      }
    },
    methods:{
      showDetail() {
        this.detailShow = true
      },
      hideDetail() {
        this.detailShow = false
      }
    },
    created() {
      this.classMap = ['decrease','discount','special','invoice','guarantee']
    },
    components:{
      star,
    }
  }
</script>

<style scoped>
 /* @import '/src/common/sell-icon/style.css';*/
 /*.clearfix:after{
   display:block;
   content:'';
   clear:both;
   height:0;
   visibility:hidden;
 }*/
 body,html{
    line-height:1;
    font-weight:200;
  }
  .header{
    position:relative;
    color:rgb(255,255,255);
    background-color: rgba(7,17,27,0.5);
    overflow:hidden;
  }
  .header .content-wrapper{
    position:relative;
    padding:24px 12px 18px 24px;
    font-size:0;
  }
  .header .content-wrapper .avatar{
    display:inline-block;
    font-size:14px;
    vertical-align:top;
  }
  .header .content-wrapper .avatar img{
    border-radius:2px;
  }
  .header .content-wrapper .content{
    display:inline-block;
    font-size:14px;
    margin-left:16px;
  }
  .header .content-wrapper .content .title{
    margin:2px 0 8px 0;
  }
  .header .content-wrapper .content .title .brand{
    display:inline-block;
    vertical-align:top;
    width:30px;
    height:18px;
    background-image: url(./brand@2x.png);
    background-size:30px 18px;
    background-repeat: no-repeat;
    blur:5px;
  }
  .header .content-wrapper .content .title .name{
    margin-left:6px;
    font-size:16px;
    line-height:18px;
    font-weight:bold;
    color:rgb(255,255,255);
  }
  .header .content-wrapper .content .description{
      margin-bottom:10px;
      color:rgb(255,255,255);
      line-height:12px;
      font-size:12px;
  }
  .header .content-wrapper .content .support{
    font-size:0;
    margin-bottom:2px;
  }
  .header .content-wrapper .content .support .icon{
      display:inline-block;
      vertical-align:top;
      width:12px;
      height:12px;
      margin-right:4px;
      background-size:12px 12px;
      background-repeat:no-repeat;
  }
  .header .content-wrapper .content .support .decrease{
      background-image:url(./decrease_1@2x.png);
  }
  .header .content-wrapper .content .support .discount{
    background-image:url(./discount_1@2x.png);
  }
  .header .content-wrapper .content .support .special{
    background-image:url(./special_1@2x.png);
  }
  .header .content-wrapper .content .support .invoice{
    background-image:url(./invoice_1@2x.png);
  }
  .header .content-wrapper .content .support .guarantee{
    background-image:url(./guarantee_1@2x.png);
  }
  .header .content-wrapper .content .support .text{
    font-size:10px;
    color:rgb(255,255,255);
    font-weight:200;
    line-height:12px;
  }
  .header .content-wrapper .support-count{
    position:absolute;
    right:12px;
    bottom:18px;
    padding:0 8px;
    height:24px;
    line-height:24px;
    border-radius:14px;
    background-color: rgba(0,0,0,0.2);
    text-align:center;
  }
  .header .content-wrapper .support-count .count{
    font-size:10px;
    margin-right:2px;
  }
  .header .content-wrapper .support-count i{
    font-size:10px;
  }
  .header .bulletin-wrapper{
    position:relative;
    height:28px;
    line-height:28px;
    padding:0 22px 0 12px;
    white-space:nowrap;
    overflow:hidden;
    text-overflow:ellipsis;
    background-color: rgba(7,17,27,0.2);
  }
  .header .bulletin-wrapper .bulletin-title{
    display:inline-block;
    width:22px;
    height:12px;
    vertical-align: top;
    margin-top:8px;
    background-image:url('bulletin@2x.png');
    background-size:22px 12px;
    background-repeat: no-repeat;
  }
  .header .bulletin-wrapper .bulletin-text{
    margin:0 4px;
    vertical-align: top;
    font-size:10px;
    font-weight:200;
  }
  .header .bulletin-wrapper i{
    position:absolute;
    top:0px;
    font-size:10px;
    right:12px;
  }
  .header .background{
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    z-index:-1;
    filter:blur(10px);
  }
  .header .detail{
    position:fixed;
    z-index:100;
    width:100%;
    height:100%;
    top:0;
    left:0;
    overflow:auto;
    background-color: rgba(7,17,27,0.8);
    /*filter:blur(10px)*/
    -webkit-backdrop-filter:blur(10px);
  }
 .fade-enter-active, .fade-leave-active {
   transition: all 0.4s
 }
 .fade-enter, .fade-leave-to {
   opacity: 0
 }
 .detail-wrapper{
    min-height:100%;
  }
 .detail-wrapper .detail-main{
    margin-top:64px;
    padding-bottom:64px;
 }
 .detail-close{
    position:relative;
    width:32px;
    height:32px;
    margin:-64px auto 0 auto;
    clear:both;
    font-size:32px;
 }
  .detail-main .name{
    line-height:16px;
    font-size:16px;
    text-align: center;
    font-weight:700;
  }
  .detail-wrapper .star-wrapper{
    margin-top:22px;
    padding-bottom:34px;
    text-align:center;
  }
 .detail-wrapper .title{
   display:flex;
   width:80%;
   margin:28px auto 24px auto;

 }
 .detail-wrapper .title .line{
   flex:1;
   position:relative;
   top:-6px;
   border-bottom:1px solid rgba(255,255,255,0.2);
 }
 .detail-wrapper .title .text{
   padding:0 12px;
   font-size:14px;
   font-weight:700;
 }
 .detail-wrapper .supports{
   width:80%;
   margin:0 auto;
 }
 .detail-wrapper .supports .support-item{
   padding:0 12px;
   margin-bottom:12px;
   font-size:0;
 }
 .detail-wrapper .supports .support-item:last-child{
   margin-bottom:0;
 }
 .detail-wrapper .supports .support-item .icon{
   display:inline-block;
   width:16px;
   height:16px;
   vertical-align:top;
   margin-right:6px;
   background-size:16px 16px;
   background-repeat: no-repeat;
 }
 .detail-wrapper .supports .support-item .decrease{
   background-image:url(./decrease_1@2x.png);
 }
 .detail-wrapper .supports .support-item .discount{
   background-image:url(./discount_1@2x.png);
 }
 .detail-wrapper .supports .support-item .special{
   background-image:url(./special_1@2x.png);
 }
 .detail-wrapper .supports .support-item .invoice{
   background-image:url(./invoice_1@2x.png);
 }
 .detail-wrapper .supports .support-item .guarantee{
   background-image:url(./guarantee_1@2x.png);
 }
 .detail-wrapper .supports .support-item .text{
   line-height:16px;
   font-size:12px;
 }
 .detail-wrapper .bulletin{
   width:80%;
   margin:0 auto;
 }
 .detail-wrapper .bulletin .content{
   padding:0 12px;
   font-size:12px;
   line-height:24px;
 }
</style>
