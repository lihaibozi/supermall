<template>
  <div>
    <navbar class="home-nav" >
      <div   slot="center">购物街</div>
    </navbar>
    <home-swiper :banners="banners"/>
  <home-recommand-vie :recommends="recommends"/>
  </div>
</template>

<script>
  import navbar from 'components/common/navbar/navbar'
  import {getHomeMultidata} from 'network/homerequest'
  import HomeSwiper from './childComps/homeswiper.vue'
  import HomeRecommandVie from './childComps/HomeRecommandView.vue'


    export default {
        name: "home",
        components:{
          navbar,
          HomeSwiper,
          HomeRecommandVie
        },
      data(){
          return{
            banners:[],
            recommends:[]
          }
      },
      created(){
          getHomeMultidata().then(res =>{
           this.banners = res.data.banner.list;
           this.recommends= res.data.recommend.list;
          })
      }
    }
</script>

<style scoped>
    .home-nav{
      background-color: var(--color-tint);
      color: #ffffff;
    }
</style>
