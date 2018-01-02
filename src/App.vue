<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/goods">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item" @click="a">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import header from './components/header/header.vue';
  import { urlParse } from './common/js/until';

  const ERR_OK = 0;
  export default {
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse();
            return queryParam.id;
          })()
        }
      };
    },
    methods: {
      a() {
        console.log(this.seller.id);
      }
    },
    components: {
      'v-header': header
    },
    created() {
      this.$http.get('/api/seller' + '?id=' + this.seller.id).then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.seller = Object.assign({}, this.seller, response.data);
        }
      });
    }
  };
</script>

<style>
  .tab{
    width: 100%;
    height: 40px;
    display: flex;
    line-height: 40px;
  }
  .tab-item{
    flex: 1;
    text-align: center;
    color: rgb(77,85,96);
    vertical-align: middle;
  }
  .tab-item a{
    display: block;
    font-size: 14px;
  }
  .tab-item a.active {
    color: rgb(240,20,20);
  }
</style>
