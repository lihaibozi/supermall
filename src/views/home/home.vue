<template>
  <div>
    <navbar class="home-nav" >
      <div   slot="center">购物街</div>
    </navbar>
    <home-swiper :banners="banners"/>
    <home-recommand-vie :recommends="recommends"/>
    <feature-view/>
    <tab-bar-control :titles="['流行','新款','精选']" class="tab-control"
                    @tabControl="tabControl"/>
    <goods-list :goods="showGoods"/>

  </div>
</template>

<script>
  import HomeSwiper from './childComps/homeswiper.vue'
  import HomeRecommandVie from './childComps/HomeRecommandView.vue'
  import FeatureView from './childComps/FeatureView.vue'

  import navbar from 'components/common/navbar/navbar'
  import TabBarControl from 'components/content/tabbarControl/TabBarControl'
  import GoodsList from 'components/content/Goods/GoodsList'


  import {getHomeMultidata} from 'network/homerequest'
  import {getHomeGoods} from 'network/homerequest'


  export default {
        name: "home",
        components:{
          navbar,
          HomeSwiper,
          HomeRecommandVie,
          FeatureView,
          TabBarControl,
          GoodsList
        },
      data(){
          return{
            banners:[],
            recommends:[],
            goods:{
              'pop':{page:0,list:[]},
              'new':{page:0,list:[]},
              'sell':{page:0,list:[]},
            },
            currentType:'pop'
          }
      },
    computed:{
      showGoods(){
        return this.goods[this.currentType].list
      }
    },
      created(){
        this.getHomeMultidata()
        this.getHomeGoods('pop')
        this.getHomeGoods('new')
        this.getHomeGoods('sell')
      },
    methods:{
       //事件监听
      tabControl(index){
        switch(index){
          case 0:
            this.currentType='pop'
            break
          case 1:
            this.currentType='new'
            break
          case 2:
            this.currentType='sell'
            break
        }
      },
      getHomeMultidata(){
        getHomeMultidata().then(res =>{
          this.banners = res.data.banner.list;
          this.recommends= res.data.recommend.list;
        })
      },
      getHomeGoods(type){
        const page = this.goods[type].page+1;
        getHomeGoods(type,page).then(res=>{
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page += 1;
        })
      }
    }
    }
</script>

<style scoped>
    #home{
      padding-top: 44px;
    }
    .home-nav{
      background-color: var(--color-tint);
      color: #ffffff;
      position: fixed;
      left: 0;
      right: 0;
      top: 0;
      z-index: 9;
    }
  .tab-control{
    position: sticky;
    top: 44px;
    z-index: 9;
    background-color:#ffffff;
  }
</style>
