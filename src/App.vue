<template>
  <div id="apps">
    <v-header :seller="seller"></v-header>
    <div class="tab line-1px">
      <div class="tab-item">
        <router-link to="/goods" replace>商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings" replace>评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller" replace>商家</router-link>
      </div>
    </div>
    <keep-alive>
      <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script>
import header from './components/header/header.vue'
import {urlParse} from './common/js/util.js';
export default {
  name: 'apps',
  data() {
    return {
        seller:{
          id:(()=>{
            let queryParam=urlParse();
            return queryParam.id;
          })()
        }
    }
  },
  created() {    
    this.$http.get('/api/seller?id='+this.seller.id).then((response) => {
      response=response.body;      
      if(response.errno===0){
        this.seller = Object.assign({}, this.seller, response.data);
      }
    })
  },
  components:{
    'v-header':header
  }
}
</script>

<style>
.tab{
  display: flex;
  width:100%;
  height:40px;
  line-height: 40px;
  text-align: center;
}
.tab .tab-item {
  flex:1; 
}
.tab .tab-item>a{
  text-decoration: none;
  color:rgb(77,85,93);
  display: block;
  font-size: 14px;
}

.tab .tab-item .active{
  color:rgb(200,20,20);
}


</style>
