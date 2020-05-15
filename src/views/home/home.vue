<template>
  <div>
    <navbar class="home-nav" >
      <div   slot="center">购物街</div>
    </navbar>
  <bt-scroll class="content" ref="scroll"
             :probeType="3" :pullUpLoad="true"
             @homeScroll="homeScroll" @pullingUp="pullingUp">
    <home-swiper :banners="banners"/>
    <home-recommand-vie :recommends="recommends"/>
    <feature-view/>
    <tab-bar-control :titles="['流行','新款','精选']" class="tab-control"
                     @tabControl="tabControl" />
    <goods-list :goods="showGoods"/>
  </bt-scroll>
  <back-top @click.native="backClick" v-show="isShowBackTop" />
  </div>
</template>

<script>
  import HomeSwiper from './childComps/homeswiper.vue'
  import HomeRecommandVie from './childComps/HomeRecommandView.vue'
  import FeatureView from './childComps/FeatureView.vue'

  import navbar from 'components/common/navbar/navbar'
  import TabBarControl from 'components/content/tabbarControl/TabBarControl'
  import GoodsList from 'components/content/Goods/GoodsList'
  import BtScroll from 'components/common/btscroll/btscroll'
  import BackTop from 'components/content/backtop/backtop'

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
          GoodsList,
          BtScroll,
          BackTop
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
            currentType:'pop',
            isShowBackTop:true
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
      homeScroll(position){
        this.isShowBackTop = (-position.y)>1000
      },
      pullingUp(){
        this.getHomeGoods(this.currentType);
        this.$refs.scroll.finishPullingUp();
      },
      backClick(){
        this.$refs.scroll.scrollTo(0,0);
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
      height: 100vh;
      position: relative;
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
  .content{
    overflow: hidden;
    position: absolute;
    top: 44px;
    bottom:49px;
    left: 0;
    right: 0;
  }
</style>
