<template>
<div>
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods" >
          商品
        </router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings" >
          评论
        </router-link>
      </div>
      <div class="tab-item" >
        <router-link to="/seller">
          商家
        </router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
</div>
</template>

<script>
  import header from './components/header/header.vue'
  import goods from './components/goods/goods.vue'
  import {urlParse} from '@/common/js/util'

  const ERR_OK = 0
  export default{
    data() {
      return{
        seller:{
          id: (()=> {
            let queryParam = urlParse()
            return queryParam.id
          })()
        }
      }
    },
    components:{
      'v-header': header
    },
    created() {
      this.$http.get('/api/seller?id='+this.seller.id).then((response)=>{
        response = response.body
        if(response.errno === ERR_OK){
          this.seller = Object.assign({},this.seller,response.data)
          console.log(this.seller.id)
        }
      })
    }
  }
</script>

<style>
  .tab{
    display:flex;
    width:100%;
    height:40px;
    line-height:40px;
    border:1px solid rgba(1,17,27,0.1);
  }
  .tab .tab-item{
    flex:1;
    text-align:center;
  }
  a .active{
    color:rgb(240,20,20);
  }
  .tab .tab-item a{
    font-size:14px;

  }
</style>
